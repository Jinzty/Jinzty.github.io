mongo支持使用collation对排序处理，可以用来排序数值字符串

    Document document = new Document(ImmutableMap.of("locale", "zh", "numericOrdering", true));
    query.collation(Collation.from(document));

但这样排序对浮点数和科学计数的字段排序结果不符合预期，还是需要转Number排序