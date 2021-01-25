# nextcloud

Install nextcloud:
```bash
kubectl create deployment nextcloud --image=nextcloud
kubectl expose deployment nextcloud --port=80 --name=nextcloud
```

Setup ingress:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: nextcloud
  annotations:
    nginx.org/websocket-services: nextcloud
spec:
  tls:
    - hosts:
      - nextcloud.k8s.shubhamtatvamasi.com
      secretName: letsencrypt
  rules:
    - host: nextcloud.k8s.shubhamtatvamasi.com
      http:
        paths:
        - path: /
          backend:
            serviceName: nextcloud
            servicePort: 80
EOF
```

Delete everything:
```bash
kubectl delete deploy/nextcloud svc/nextcloud ing/nextcloud
```

