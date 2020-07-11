kubernetes-action
=============
Interacts with kubernetes clusters calling `kubectl` commands. Integrates support for **AWS EKS**.

## Usage

### Basic Example

```yml
name: CI

on:
  - push

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Trigger deploy
        uses: wMw9/kubernetes-action@master
        env:
          KUBE_CONFIG_DATA: ${{ secrets.KUBE_CONFIG_DATA }}
        with:
          args: apply -f path/to/deployment.yaml
```

## Config

### Secrets

One or more **secrets** needs to be created to store cluster credentials. (see [here](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) for help on creating secrets). 

#### Basic
- **KUBE_CONFIG_DATA**: A `base64` representation of `~/.kube/config` file.

##### Example
```bash
cat ~/.kube/config | base64 | pbcopy # pbcopy will copy the secret to the clipboard (Mac OSX only)
```

## Outputs

- **result**: Output of the `kubectl` command.

### Example
```yaml
      - name: Save container image
        id: image-save
        uses: Consensys/kubernetes-action@master
        env:
          KUBE_CONFIG_DATA: ${{ secrets.KUBE_CONFIG_DATA }}
        with:
          args: get deploy foo -o jsonpath="{..image}"

      - name: Print image
        run: 
          echo ${{ steps.image-save.outputs.result }}
```

More info on how to use outputs [here](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions#outputs).
