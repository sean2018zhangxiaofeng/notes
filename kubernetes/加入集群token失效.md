登陆master使用kubeadm token list都查看token状态
如果token失效
使用kubeadm token create创建新的token，默认失效只有24小时
使用命令
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
获取sha256编码hash值

kubeadm join [masterip]:6443 --token [token] --discovery-token-ca-cert-hash [sha256编码hash值]