## 爬坑spring
### spring 由来
### spring 核心
- ioc 控制反转
- aop 切面编程
### spring优点：
1. 方便解耦简化开发<br>
   spring 就是一个大工厂可以将所有对象创建和依赖关系维护，交给spring管理
2. AOP 编程的支持，
   <br>SPRING 面向切面编程可以方便实现对程序进行权限拦截，运行监控等功能
3. 声明式事务支持：
   <br>
4. 方便程序测试：
5. 方便集成各种优秀框架
6. 降低javaEE API使用难度
### 高内聚低耦合
1. 一样的东西尽量放到一起
2. 能不调用尽量少调用
3. spring 工厂用于生产bean
### 一个简单的IOC
实现一个简单的ioc 大概包含以下4步、
1. 加载xml文件，遍历里面的标签
2. 获取标签中的id和class属性，
3. 加载属性相关的类并创建bean
4. 将bean注册到bean容器中
代码：
```java
public class SimpleIOC {
    private Map<String, Object> beanMap = new HashMap<String, Object>();

    public SimpleIOC(String location) throws Exception {
        loadBeans(location);
    }

    public Object getBean(String name) {
        Object bean = beanMap.get(name);
        if (bean == null) {
            throw new IllegalArgumentException("there is no bean with name " + name);
        }
        return bean;
    }

    private void loadBeans(String location) throws Exception {
        // 加载 xml 配置文件
        InputStream inputStream = new FileInputStream(location);
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = factory.newDocumentBuilder();
        Document doc = docBuilder.parse(inputStream);
        Element root = doc.getDocumentElement();
        NodeList nodes = root.getChildNodes();

        // 遍历 <bean> 标签
        for (int i = 0; i < nodes.getLength(); i++) {
            Node node = nodes.item(i);
            if (node instanceof Element) {
                Element ele = (Element) node;
                String id = ele.getAttribute("id");
                String className = ele.getAttribute("class");
                // 加载 beanClass
                Class beanClass = null;
                try {
                    beanClass = Class.forName(className);
                } catch (ClassNotFoundException e) {
                    e.printStackTrace();
                    return;
                }

                // 创建 bean
                Object bean = beanClass.newInstance();

                // 遍历 <property> 标签
                NodeList propertyNodes = ele.getElementsByTagName("property");
                for (int j = 0; j < propertyNodes.getLength(); j++) {
                    Node propertyNode = propertyNodes.item(j);
                    if (propertyNode instanceof Element) {
                        Element propertyElement = (Element) propertyNode;
                        String name = propertyElement.getAttribute("name");
                        String value = propertyElement.getAttribute("value");

                        // 利用反射将 bean 相关字段访问权限设为可访问
                        Field declaredField = bean.getClass().getDeclaredField(name);
                        declaredField.setAccessible(true);

                        if (value != null && value.length() > 0) {
                            // 将属性值填充到相关字段中
                            declaredField.set(bean, value);
                        } else {
                            String ref = propertyElement.getAttribute("ref");
                            if (ref == null || ref.length() == 0) {
                                throw new IllegalArgumentException("ref config error");
                            }
                            // 将引用填充到相关字段中
                            declaredField.set(bean, getBean(ref));
                        }
                        // 将 bean 注册到 bean 容器中
                        registerBean(id, bean);
                    }
                }
            }
        }
    }

    private void registerBean(String id, Object bean) {
        beanMap.put(id, bean);
    }
}
```
### 简单的AOP