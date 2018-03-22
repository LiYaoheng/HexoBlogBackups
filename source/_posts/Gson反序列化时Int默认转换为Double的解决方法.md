---
title: Gson反序列化时Int默认转换为Double的解决方法
date: 2018-03-22 15:00:46
tags: Java
---

项目中使用Gson进行json的转换，碰到的问题：当json窜反序列化的时候，出现了Int默认转换为Double的问题，原因是Gson的源码中对number数据默认都处理为了double。

**截取源码部分关键代码**

``` Java
case STRING:
 return in.nextString();
//数值类型全部转换为了Double类型
case NUMBER:
 return in.nextDouble();
case BOOLEAN:
 return in.nextBoolean();
```

----

**解决思路：**
> 1. 如果反序列化的类型确定，**直接指定Entity类型**。
> 2. 注册类型适配器，**重写map的反序列化**（修改源码中case NUMBER的逻辑），[这篇文章给出了重写的代码](http://www.lidetao.com/java-gson-json2map-int2double.html/comment-page-1#comment-2368)

----

**解决方法实现：**
> 推荐阅读[这篇文章](http://www.lidetao.com/java-gson-json2map-int2double.html/comment-page-1#comment-2368)

**注：jackson中没有这个问题。。。**


-----

>  <span id="codeHead">GsonUtils</span>代码做个备份。摘抄于上面推荐阅读的文章。

``` Java
public class GsonUtil {

    /**
     * 序列化时，null列不处理问题，添加serializeNulls()。
     */
    private static Gson gson = new GsonBuilder().serializeNulls().create();

    public static String toJson(Object data) {
        return gson.toJson(data);
    }

    /**
     * 反序列化时
     * 1、int 转变为 double 问题，重写map适配器，判断number逻辑
     * @return
     */
    //调用：List<Map<String,Object>> jsonList = GsonUtil.fromJson(json,new TypeToken<List<Map<String,Object>>>(){});
    public static <T> T fromJson(String json, TypeToken<T> typeToken){
        Gson gson = new GsonBuilder().serializeNulls().registerTypeAdapter(new TypeToken<Map<String,Object>>(){}.getType()
		,new MapTypeAdapter()).create();
        return gson.fromJson(json,typeToken.getType());
    }

    public static class MapTypeAdapter extends TypeAdapter<Object> {
        @Override
        public Object read(JsonReader in) throws IOException {
            JsonToken token = in.peek();
            switch (token) {
                case BEGIN_ARRAY:
                    List<Object> list = new ArrayList<>();
                    in.beginArray();
                    while (in.hasNext()) {
                        list.add(read(in));
                    }
                    in.endArray();
                    return list;

                case BEGIN_OBJECT:
                    Map<String, Object> map = new LinkedTreeMap<>();
                    in.beginObject();
                    while (in.hasNext()) {
                        map.put(in.nextName(), read(in));
                    }
                    in.endObject();
                    return map;

                case STRING:
                    return in.nextString();

                case NUMBER:
                    /**
                     * 改写数字的处理逻辑，将数字值分为整型与浮点型。
                     */
                    double dbNum = in.nextDouble();

                    // 数字超过long的最大值，返回浮点类型
                    if (dbNum > Long.MAX_VALUE) {
                        return dbNum;
                    }

                    // 判断数字是否为整数值
                    long lngNum = (long) dbNum;
                    if (dbNum == lngNum) {
                        return lngNum;
                    } else {
                        return dbNum;
                    }

                case BOOLEAN:
                    return in.nextBoolean();

                case NULL:
                    in.nextNull();
                    return null;

                default:
                    throw new IllegalStateException();
            }
        }

        @Override
        public void write(JsonWriter out, Object value) throws IOException {
            // 序列化无需实现
        }

    }
}
```  

 **END [再看一遍代码](#codeHead)**
