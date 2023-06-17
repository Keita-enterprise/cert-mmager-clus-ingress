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
