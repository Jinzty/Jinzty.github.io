资源同步有些存在标签属性，有些又没有
因标签是通用查询项，update更新不进去标签到mongo
为保证查询一致，入库到mongo前，对无标签属性的资源加一层资源与标签实现

public class ResourceWithTag<R, T> {
    private R r;
    private List<T> tags;

    public ResourceWithTag(R r, List<T> tags) {
        this.r = r;
        this.tags = tags;
    }

另外入库mongo后对应class或包名变动导致找不到自动装箱的情况查询出来作为Map类型处理
拷贝属性不能使用BeanUtils.copyProperties(source, target)而要使用BeanUtils.populate(bean, map)

                        if (msgData.getMsg() instanceof ResourceData) {
                            dataVo = convertResourceNotag((ResourceData) msgData.getMsg(), accountAliasNameTable);
                        } else {
                            ResourceData rn = new ResourceData();
                            BeanUtils.populate(rn, (Map) msgData.getMsg());
                            dataVo = convertResourceNotag(rn, accountAliasNameTable);
                        }

消息提醒要展示的数据入库mongo先转换好，避免取出时候再转换

针对一些远程查询的数据结果放入mongo，并对查询操作做redis缓存处理（根据查询处理成功与否根据业务判断是否缓存）