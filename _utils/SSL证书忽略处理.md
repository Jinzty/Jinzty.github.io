研发时候直接使用ip联调httpClient调用报错证书不属于指定域
调整判断忽略SSL处理

    SSLContext sslContext = SSLContexts.custom().loadTrustMaterial(null, (chain, authType) -> true).build();
    return HttpClients.custom().setSSLContext(sslContext).setSSLHostnameVerifier(new NoopHostnameVerifier()).build();