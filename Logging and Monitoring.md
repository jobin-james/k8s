## Logging and monitoring
- metric servers, elk, datadog etc
- Metric server is an in memory monitoring solution
- Enable metric server on `minikube installation` `minikube addons enable metric-server`
- Enable metric server on Kubernates installation  
  - `git clone https://github.com/kubernates-incubator/metrics-server.git`
  - `kubectl apply -f metric-server/`
  
- check for node status `kubectltop node`
- check for pods `kubectltop pod`

### managing application logging
- `kubectl logs -f <pod name>`
