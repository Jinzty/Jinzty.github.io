只要在ip段内的ip/前缀位构成的cidr表示的ip段都是一样的，故正常会对一个不规范的cidr简化成对应ip段最小值/前缀位
```
public String simplifyIpv4(String cidrIp) {
    String[] cidr = cidrIp.split("/");
    if (cidr.length < 2) {
        return cidrIp + "/" + 32;
    }
    String strIp = cidr[0];
    long cidrPrefix = Long.parseLong(cidr[1]);
    long ipLong = IpUtils.ipToLong(strIp);
    long suffix = 32 - cidrPrefix;
    long lowestAddress = ipLong >>> suffix << suffix;
    return IpUtils.longToIp(lowestAddress) + "/" + cidrPrefix;
}
```
