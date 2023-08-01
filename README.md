# Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO

This is me exploring the deployment of Nethermind's Ethereum Client with monitoring tools.

Most of the content of this repo was initially cloned from [NethermindEth](https://github.com/NethermindEth/terraform-nethermind)

## Deployment Notes

1. Provisioner Block
   
I deployed on Digital Ocean, so I had to add the Digital Ocean Provider block to the main.tf file. I didn't realize it wasn't added in the cloned repo and only discovered it after my deployment threw up an error at ```terraform init``` stage.

   ```
   terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "~> 2.0"
    }
  }
}
```
<img width="651" alt="neth terra init" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/0fcc5961-806b-46da-924a-01b051a3c9c1">


2. Process exiting
At the ```terraform apply``` stage my deployment process exited.

<img width="817" alt="neth terra apply timing out with error" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/53ab6d49-3288-420a-834d-1f93d48dfc22">


To debug the cause of this, I checked the deployment flow log and discovered that an apt-get process was holding the deployment so it timed out. 

<img width="947" alt="neth terra apply timing out process held" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/fd2c51c0-f848-49f0-a4a0-909fd0d299bb">


I went to the remote exec provisioner block in main.tf file and changed all ```apt-get``` to ```apt```. I can't explain how exactly this fixed my issue, but it did.


* Terraform applied successfully
  
<img width="878" alt="neth terra apply successful" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/e6c00a36-834a-4475-b26c-5cdf70dcf6ff">

* Ethereum server deployed
<img width="769" alt="neth droplet" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/4b14d553-3060-45da-9f51-468876e46ead">

* Prometheus Running
<img width="956" alt="neth Prometheus" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/0ab4c980-4efb-4b03-bb07-18223afca9fc">

* Grafana Running
<img width="520" alt="neth Grafana login" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/a52a232f-12b3-4da0-acc9-ed63538af42d">

* Prometheus Pushgateway running
<img width="769" alt="neth Pushgateway" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/40219d6f-08b9-4102-a2d1-d085393aa665">

* Seq Running
<img width="954" alt="neth Seq" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/a0e24b0a-cd9f-4fe5-a066-021dfcb294bf">

* Terraform Destroy
<img width="570" alt="neth terra destroy" src="https://github.com/Osinachi-Uro/Terraform-to-Deploy-Nethermind-Ethereum-Client-with-Monitoring-on-DO/assets/83463641/757bbe86-70b2-4cbe-9713-7f3f10adcdf7">


