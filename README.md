# Steps by steps installation 
# 1- Install Cert-manager 
      helm repo add jetstack https://charts.jetstack.io
      helm install my-release --namespace cert-manager --version v1.12.2 jetstack/cert-manager
# 2 - Create clusterissuer file 
      apiVersion: cert-manager.io/v1
      kind: ClusterIssuer
      metadata:
        name: letsencrypt-prod
      spec:
        acme:
          server: https://acme-v02.api.letsencrypt.org/directory
          email: keibk@protonmail.com
          privateKeySecretRef:
            name: letsencrypt-prod
          solvers:
          - http01:
              ingress:
                class: nginx
# 3- Create a file for Ingress
            apiVersion: networking.k8s.io/v1
            kind: Ingress
            metadata:
              name: example-ingress
              annotations:
                kubernetes.io/ingress.class: nginx
                cert-manager.io/cluster-issuer: letsencrypt-prod
            spec:
              tls:
              - hosts:
                - prometheus.samadounoorcloudsolutions.com
                secretName: example-tls-secret
              rules:
              - host: prometheus.samadounoorcloudsolutions.com
                http:
                  paths:
                  - path: /
                    pathType: Prefix
                    backend:
                      service:
                        name: prometheus-server
                        port:
                          number: 80
