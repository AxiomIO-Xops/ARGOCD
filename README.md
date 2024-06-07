# Set Up Continuous Deployment to AKS using ArgoCD

**Repository**: [argocd](https://github.com/pkrishnamohan007/argocd.git)
1. **Create secret**
   Create secret for argocd repository.
   ```bash
   kubectl apply -f repo-creds/repo-cred.yml
   ```
2. **Bootstrap ArgoCD**

   Install and configure ArgoCD in your AKS cluster using the `bootstrap` directory.

   ```bash
   kubectl apply -k bootstrap/
   ```

3. **Configure ArgoCD Applications**

   Define applications in `apps/app1` using kustomize for environment-specific overlays (e.g., `prod`, `stage`).

   Example `kustomization.yml` for `prod`:

   ```yaml
   resources:
     - deployment.yml
     - service.yml
   ```

4. **Deploy Applications Using ArgoCD**

   Create an ArgoCD application pointing to the `argocd` repository.
   
   **eaxmple yml file**

   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Application
   metadata:
     name: app1
     namespace: argocd
   spec:
     project: default
     source:
       repoURL: 'https://github.com/pkrishnamohan007/argocd.git'
       targetRevision: HEAD
       path: apps/app1/overlays/prod
     destination:
       server: 'https://kubernetes.default.svc'
       namespace: default
     syncPolicy:
       automated:
         prune: true
         selfHeal: true
   ```
