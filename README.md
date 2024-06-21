# play.catalog
Play Economy Catalog microservice

## Create and publish contract package
```powershell
$version="1.0.1"
$owner="ISoftt"
$github_personal_access_token="specify here"

dotnet pack src\Play.Catalog.Contract\ --configuration Release -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/play.catalog -o ..\packages

dotnet nuget push ..\packages\Play.Catalog.Contract.$version.nupkg --api-key $github_personal_access_token --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="ISoftt"
$env:GH_PAT="[PAT HERE]"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t play.catalog:$version .
```

## Run the docker image
```powershell
docker run -it --rm -p 5000:5000 --name catalog -e MongoDbSettings__Host=mongo -e RabbitMQSettings__Host=rabbitmq --network playinfra_default play.catalog:$version
```