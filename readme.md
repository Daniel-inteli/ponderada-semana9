# Como criar uma EC2 com Terraform

## Pré requisitos

<li> Conta na AWS
<li> Terraform CLI
<li> AWS CLI

## Instalando Terraform CLI

##### Abrir Powershell como administrador

Para instalar novos programas é necessário abrir o powershell como administrador

##### Baixar Terraform

No Powershell, adicione o comando abaixo para baixar o aquivo ZIP.
`Invoke-WebRequest -Uri https://releases.hashicorp.com/terraform/1.0.11/terraform_1.0.11_windows_amd64.zip -OutFile terraform.zip` 

##### Extrair o aquivo ZIP

Extraia o aquivo com o comando: `Expand-Archive -Path terraform.zip -DestinationPath C:\terraform
`

##### Adicionar o Terraform ao PATH

Para poder executar o Terraform a partir de qualquer lugar ou terminal, digite o comando abaixo para adicionar o Terraform como variável de ambiente
`[System.Environment]::SetEnvironmentVariable('PATH', $env:PATH + ';C:\terraform', [System.EnvironmentVariableTarget]::Machine)
`
Para verificar se tudo ocorreu corretamente, abra outro terminal e adicione o seguinte comando para verificar a versão do Terraform: `terraform -v
`

## Instalando AWS CLI

##### Abrir Powershell como administrador

Para instalar novos programas é necessário abrir o powershell como administrador

##### Baixar AWS CLI

No Powershell, adicione o comando abaixo para baixar o aquivo msi.
`Invoke-WebRequest -Uri https://awscli.amazonaws.com/AWSCLIV2.msi -OutFile AWSCLIV2.msi`

##### Instalar o AWS CLI

Com o comando `Start-Process msiexec.exe -Wait -ArgumentList '/I AWSCLIV2.msi /quiet'` o AWS CLI é instalado, caso haja algum problema, você pode executar o arquivo direto da pasta onde foi baixado.

Para verificar se foi instalado, assim como no passo do Terraform, basta abrir um terminal e executar o comando `aws --version
` 

##### Configurando as credencias AWS
Execute o comando `aws configure
` e insira as credencias da sua conta AWS.

Se você está utilizando uma conta do AWS Learner Lab, você irá precisar do AWS Session Token.
Ainda no powershell digite `cd ~\.aws\
`, depois `notepad credentials
` para abrir o bloco de notas com as credenciais, você irá alterar pelas credencias da sua sessão no learner lab, que você pode identificar na imagem abaixo.


![Image](/assets/Credentials.png)

## Criando a instância EC2
##### Arquivo Main.tf

No seu repositório crie um arquivo de configuração `main.tf`  com o seguinte conteúdo:

<code>
provider "aws" {
  region = "us-east-1"
}
resource "aws_instance" "example" {
  ami           = "ami-0c02fb55956c7d316"  # Amazon Linux 2 AMI (HVM), SSD Volume Type
  instance_type = "t2.micro"

  tags = {
    Name = "TerraformExample"
  }
}
</code>

![Image](/assets/main.png)

##### Inicializando Terraform

Abra o terminal e digite `terraform init` e execute.
Este comando inicializa o diretório de trabalho contendo os arquivos de configuração do Terraform. Ele baixa os plugins necessários para interagir com os provedores de serviços declarados.

![Image](/assets/init.png)
##### Planejando a Infraestrutura

Digite e execute `terraform plan`.
Este comando cria um plano de execução, mostrando as ações que o Terraform realizará para alcançar o estado descrito na configuração. Assim você consegue visualizar as mudanças antes de aplicá-las.

![Image](/assets/plan.png)

##### Criando a instância
Digite e execute `terraform apply`.
Este comando aplica as mudanças necessárias para alcançar o estado desejado na configuração. O Terraform pedirá confirmação antes de aplicar as mudanças. Digite yes quando solicitado para confirmar a aplicação.

![Image](/assets/apply.png)

##### Bonûs: Destrua tudo
Após conseguir subir a EC2, caso você não queira mais utilizar a instância e queira evitar custos da AWS, você pode destruir a instância com o comando `terraform destroy`. Digite yes quando solicitado para confirmar a aplicação.

![Image](/assets/destroy.png)