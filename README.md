# Create namespace
1. run ```kubectl 01-sw-namespace.yaml```

# Install skywalking-8.3.0
## Create elasticsearch-7.13.0
1. run ```kubectl apply -f es/```

## Install skywalking-oap-server-8.3.0-es7
1. create static ip on Azure, refer to [https://docs.azure.cn/zh-cn/aks/ingress-basic](https://docs.azure.cn/zh-cn/aks/ingress-basic)
2. Configure the loadBalancerIP as the ip created in step 1
3. run ```kubectl apply -f oap/```

## Install skywalking-ui-8.5.0
1. run ```kubectl apply -f ui/```

## Install ingress controller
1. create static ip on Azure
2. create ingress controller based on the static ip created in step 1

- Note: Step 1 and Step 2 refer to [https://docs.azure.cn/zh-cn/aks/ingress-basic](https://docs.azure.cn/zh-cn/aks/ingress-basic)

# How to test
## Configure skywalking ui
1. Configure skywalking ui in /etc/hosts, add ```[IP] ui.sw```
- Notes: The IP is created in [Install ingress controller](Install ingress controller)

## Access to the skywalking ui
1. input "http://int.sw" in web browser

## Create skywalking sidecar base on apache-skywalking-apm-bin-es7
1. cd sw-sidecar
2. run ```docker build . --no-cache --rm -t [acr url]/skywalking/skywalking-agent-es7-sidecar:8.3.0```
3. run ```docker push [acr url]/skywalking/skywalking-agent-es7-sidecar:8.3.0```

## Create a test service via SpringBoot
1. Refer to the [sw-b](https://github.com/yumingtao/sw-b.git)

## Deploy swb
1. Set SW_AGENT_COLLECTOR_BACKEND_SERVICES as the ip create in [Install skywalking-oap-server-8.3.0-es7](Install skywalking-oap-server-8.3.0-es7) step 1 in 02-swb-deployment.yaml
2. run ```kubectl apply -f swb-test/```

## Test customized domain name
1. Configure swb access in /etc/hosts, add ```[IP] int.swb```
2. Use postman or input "http://int.swb/swb/api/v1/hello/xxxxx" in web browser
3. Check the skywalking ui