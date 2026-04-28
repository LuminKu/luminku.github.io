# 一、Java日志框架概述

Java中的Logging（日志记录）是Java平台自带的官方日志框架，它提供了一套标准化的接口和类，用于记录程序的运行状态、调试信息和错误报告。‌

核心组件
Java Logging框架包含以下几个关键组件：
  ‌Logger（日志记录器）‌：主要的日志记录入口，负责生成和发送日志消息。‌
  ‌Handler（处理器）‌：决定日志的输出目标，如控制台、文件或网络。‌
  ‌Level（级别）‌：定义日志的严重程度，包括SEVERE、WARNING、INFO、CONFIG、FINE、FINER、FINEST等。‌
  ‌Formatter（格式化器）‌：负责将日志记录格式化为可读的文本或XML格式。‌


主要特点
‌标准化‌：自Java 1.4版本引入，是Java标准库的一部分‌

‌灵活配置‌：支持静态配置（需重启生效）和动态配置（实时生效）‌

‌细粒度控制‌：可以按包名、类名设置不同的日志级别‌
‌可扩展性‌：支持自定义Handler和Formatter‌
示例代码

```java
import java.util.logging.*;

public class LoggingExample {
    private static final Logger logger = Logger.getLogger(LoggingExample.class.getName());
    public static void main(String[] args) {
        logger.setLevel(Level.ALL);
        ConsoleHandler handler = new ConsoleHandler();
        handler.setLevel(Level.ALL);
        logger.addHandler(handler);

        logger.severe("This is a SEVERE message");
        logger.warning("This is a WARNING message");
        logger.info("This is an INFO message");
        logger.fine("This is a FINE message");
    }
}
```
在Java编程中,日志(Logging)是记录程序运行信息的一种重要手段,它有助于调试、监控和记录应用程序的运行情况。


# 二、Java中JUL是什么
Java Util Logging（JUL）是Java原生的日志框架，无需引入第三方依赖，适用于小型应用。以下是核心要点：

基本概念
  JUL是Java内置的日志系统，通过java.util.logging包实现，提供日志记录、级别控制和输出管理功能 。 ‌
核心组件
  Logger：日志记录器，负责生成日志信息，支持父子继承关系（子Logger继承父Logger的配置） 。 ‌
  Handler：处理器，定义日志输出目的地（如控制台、文件），每个Logger可关联多个Handler 。 ‌
  Formatter：格式化器，控制日志输出格式（如SimpleFormatter） 。 ‌
日志级别
  从高到低：SEVERE > WARNING > INFO > CONFIG > FINE > FINER > FINEST 。 ‌
配置方式
  硬编码：直接在代码中设置日志级别、Handler等（不推荐） 。 ‌
  配置文件：通过外部文件调整日志行为（如logging.properties） 。 ‌‌
使用场景
  适用于小型项目，主流项目多采用Log4j或Logback等第三方框架 。 ‌