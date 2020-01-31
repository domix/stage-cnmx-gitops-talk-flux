# Instalación de Flux

## Instalar el utilitario CLI (OS X)

```
brew install fluxctl
```


## Helm


## Repo de flux  (Una sola vez)

```
helm repo add fluxcd https://charts.fluxcd.io
```

### Actualizar los repos de Helm (100% recomendado)

```
helm repo update
```

### Instalar los CRDs de Flux

```
kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/master/deploy/flux-helm-release-crd.yaml
```


## Repositorio de configuración

Flux necesita un repositorio de Git, en el cual ba a tomar la configuración.

Ese repo debe tener una estructura definida, si no la tiene, no funcionará.

Se puede tomar como base el siguiente repositorio:

```
https://github.com/CirculoSiete/flux-base
```

## Instalación del Operador



```
export GIT_REPO=....
```

Un Ejemplo:

```
export GIT_REPO=git@github.com:domix/stage-cnmx-gitops-talk-flux
```

### Crear el namespace flux si no existe

```
kubectl create ns flux
```


```
helm upgrade -i flux \
--set helmOperator.create=true \
--set helmOperator.createCRD=false \
--set git.url=${GIT_REPO} \
--set git.interval="10s" \
--namespace flux \
fluxcd/flux
```


### Otorgar permiso al repositorio de Git

#### Obtener llave SSH del operador


```
fluxctl identity --k8s-fwd-ns flux
```

La llave SSH que arroje el comando anterior, hay que agregarla como `Deployment Key` en el repositorio de Git, es clave que deba tener permiso de escritura.

### Parámetros interesantes al instalar Flux

[Configuración](https://github.com/fluxcd/flux/blob/master/chart/flux/README.md#configuration)