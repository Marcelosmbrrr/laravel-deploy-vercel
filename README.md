Este guia descreve o processo completo para configurar e fazer o deploy de uma aplicação Laravel 11 na Vercel. Siga os passos abaixo.

### Na raiz do seu projeto Laravel, crie uma pasta chamada `api`. Dentro desta pasta, crie um arquivo chamado `index.php` e insira o seguinte código:

```
<?php

require __DIR__ . "/../public/index.php";
```

### Na raiz do projeto, crie o arquivo .vercelignore e escreva:

```
/vendor
```

### Ainda na raiz do projeto, crie o arquivo vercel.json com esse código. O atributo APP_URL deve receber o nome do seu domínio.

```
{
    "version": 2,
    "framework": null,
    "functions": {
        "api/index.php": {
            "runtime": "vercel-php@0.6.1"
        }
    },
    "routes": [
        { "src": "/build/(.*)", "dest": "/public/build/" },
        { "src": "/(.*)", "dest": "/api/index.php" }
    ],
    "env": {
        "APP_ENV": "production",
        "APP_DEBUG": "true",
        "APP_URL": "https://your-domain.vercel.app/",
        "APP_CONFIG_CACHE": "/tmp/config.php",
        "APP_EVENTS_CACHE": "/tmp/events.php",
        "APP_PACKAGES_CACHE": "/tmp/packages.php",
        "APP_ROUTES_CACHE": "/tmp/routes.php",
        "APP_SERVICES_CACHE": "/tmp/services.php",
        "VIEW_COMPILED_PATH": "/tmp",
        "CACHE_DRIVER": "array",
        "LOG_CHANNEL": "stderr",
        "SESSION_DRIVER": "cookie"
    }
}
```

### No arquivo app/Providers/AppServiceProvider.php, faça:

```
public function boot(): void
{
    if (app()->environment('production')) {
        URL::forceScheme('https');
    }
}
```

### Vercel - Configurações do projeto:

- Alterar "Output Directory" para "public", sem aspas;
- Criar a env "APP_KEY", na seção "Environment Variables" - bem como as outras, posteriormente, necessárias para o projeto.
- Para evitar bugs, pode ser necessário alterar a versão do NodeJS para 18, que por padrão é a 20.
- Se o nome do domínio for alterado, na seção "Domains", será necessário fazer o mesmo no atributo APP_URL do arquivo vercel.json.

<!--
### Supabase - Storage:

- Instalar o pacote:

```
composer require league/flysystem-aws-s3-v3 "^3.0" --with-all-dependencies
```

- Na Vercel, criar ou editar a variável de ambiente FILESYSTEM_DISK com o valor "s3";

```
FILESYSTEM_DISK=s3
```
  
- No Supabase, criar um storage;
- Ir em "Storage Settings". Nessa seção, os dados necessários são "S3 Connection" e "S3 Access Keys".
- S3 Connection:
    - Na Vercel, criar ou editar as variáveis de ambiente AWS_DEFAULT_REGION e AWS_BUCKET, que irão receber, respectivamente, os valores do campo "Endpoint" e "Region" do painel "S3 Connection".
- S3 Access Keys:
    - Criar uma nova chave de acesso clicando em "New Access Key" e guardar os seus dados.
    - Na Vercel, criar ou editar as variáveis de ambiente AWS_ACCESS_KEY_ID e AWS_SECRET_ACCESS_KEY, que, respectivamente, é o ID E O SECRET_ID da chave S3 criada.

```
AWS_ACCESS_KEY_ID= ID DA CHAVE S3
AWS_SECRET_ACCESS_KEY= SECRET ID DA CHAVE S3
AWS_DEFAULT_REGION= REGION
AWS_BUCKET= ENDPOINT
AWS_ENDPOINT= NOME DO BUCKET
AWS_USE_PATH_STYLE_ENDPOINT=false
```
-->

