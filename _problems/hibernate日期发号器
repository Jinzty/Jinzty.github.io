有时候业务需要根据日期发号id，扩展hibernate的IncrementGenerator实现自定义的日期发号器，注解使用自定义的日期发号器

当表id下一值小于当天的第一个号时候，开始发当天的第一个号
当表id下一值在当天号范围内，直接使用下一值
当表id下一值超出当天的号段溢出时候，号段加长一位重新发第一个号

```
public class DateNoGenerator implements IdentifierGenerator, Configurable {
    ...
    @Override
    public synchronized Serializable generate(SharedSessionContractImplementor session, Object object) throws HibernateException {
        ...
        Long nextVal = (Long) previousValueHolder.makeValueThenIncrement();
        Long today = Long.parseLong(LocalDateTime.now().format(DateTimeFormatter.ofPattern(pattern)));
        return getNo(nextVal, today, 10000L);
    }
    
    private Long getNo(Long nextVal, Long today, Long pow) {
        Long todayStartNo = today * pow + 1;
        if (todayStartNo > nextVal) {
            return todayStartNo;
        }
        Long todayEndNo = (today + 1) * pow - 1;
        if (todayEndNo >= nextVal) {
            return nextVal;
        }
        return getNo(nextVal, today, pow * 10);
    }
}
```
