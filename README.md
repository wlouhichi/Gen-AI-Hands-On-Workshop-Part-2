# Introduction

In this workshop we will be learning how to set up a RAG application to process instruction PDF files and interact with the chat application.

Note: This workshop assumes that you are familiar with basics of databricks such as compute, notebooks etc.. and have a working environment with all required privileges.


## Prep work

**Clone Github:**

- Clone the git repository in databricks workspace: 

<https://github.com/wlouhichi/Gen-AI-Hands-On-Workshop-Part-2>

Note: Please use any compute in your workspace or you can use serverless compute

- Verify all the folders exist in your workspace and all the files are cloned.

- Before running the first notebook entirely, make sure the right cluster is available to run the notebooks. To setup the cluster run the following commands available in the first cell of the first notebook:

  - !pip install dbdemos 

  - import dbdemos

  - `dbdemos.create_cluster('llm-rag-chatbot')`

**Create or use existing Catalog:** 

- Create a new catalog . I used the catalog name “llm\_workshop\_2”. You can choose any name

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfH9KGl3GVrKx50bfMviwd62Qab-JnmchQl5lWQoWxTGzhWKCBnfR8ciRvFfBXr0ebsUg5u7uO2rv1D0WPRnMbQUadjf6fWK3Azcenk-Aia8iIRAXPCAT_kyMaBXjnpzHeNSDjmY_d2w59m1wod5NU?key=UNvoNqEwC0UYO-mNBhAdHukz)

- Click continue and finish setup prompts and you be presented with the screen below after creating the catalog

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXewUOH_yVNcrxhJ9uXxoH_gerDYSnKYpbtXwQYcLSSyJmVSgjtrpmYBBR0TYJ2FTCpQ462PfurE2eF_m8ASmGianc7GOGxaEsYxwjsUDaljRcgyk_1QIx5ShoAMpOGDlW2JwuHXPG0i2QQ7-ybBRY4?key=UNvoNqEwC0UYO-mNBhAdHukz)

- Create a schema by clicking on create schema. Choose a name i select “llm\_rag\_schema”

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfJhYPOFpuoExyza36sIIk7Zo_uX2DXmQa2_i-4Jw_DJ9iccV00phiNhCkfEdt55gKheKJd0o1ZwEy5L3JN_Abg99ei-2jLKPJF17_ECCuihsFtdtyx7xM72vYlV2usq-H5uuJ0kCl282HX004fjg?key=UNvoNqEwC0UYO-mNBhAdHukz)

**Create Volume**

- Click on create volume. I select name “pdfdata” you can choose any name you like

* Verify you can see the catalog, schema and volume

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXch0VD8ncv-ERNcq3YEbCcdHXtJHxLgraB1EHSMPMvzTqJpjv70TNu39guCBl7tlmgwsxUsXhLVbIbHdbUoojYdzQYT7t2X8Q8VCAdOVGpq7KzeFwcHQNhD3fJLW0pCkgdeiXd4iMWdBwHX9FPYHTo?key=UNvoNqEwC0UYO-mNBhAdHukz)

**Create a Vector Search Endpoint or use an existing one:** 

In your databricks workspace, go to compute in the left menu, then select vector search as shown below: 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdstTX_lAxe87XJcaOcfqOPxBJLdOnDXwGV-DL2wziw2yjI56SjlzamEI1cIB4VQcYXw0UILBOSrxMtY_1ShihXhZ1-J_ZUG9jddgnY5hy1Mtu4y9TCUci0ssTU4ebfDvLZBWnUpZh_h4UQy-F1U8g?key=UNvoNqEwC0UYO-mNBhAdHukz)

You can use an existing one or create a new vector search endpoint by selecting create, adding the name for the endpoint and typing confirm. I chose the name: ‘one-env-shared-endpoint-17’![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXck0VNJnLi_CE10IYod85sqF8L6NMfEJ6ndZ8JCljl_kAb6n9T7K7ZcUg2jh9oNSES8FIkYjKf_AVyiSHLGyLz11FZLSXZsk8q4k4gvN7l4XgqfhchAwWNStmrPhpxar3D6o0mVWc6kQc10h7prL1A?key=UNvoNqEwC0UYO-mNBhAdHukz)

\


In your workspace in the folder created from github, go to the file named ‘config’

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdJkRrjH7TVIYKYuSRYbKUYbrFEG-Q0Ajm-JPZ6_6aC6WsA6B4GQPN8O7m5qSW7xcWmWSBI9DaitHCtaiZlQS09lBxMDQa9rVfNXnclSOa4l0oDgscIOczVAPutHFOQLeTGHbKpATTb7JkN1zpMBzo?key=UNvoNqEwC0UYO-mNBhAdHukz)

Make sure the variables in config correspond to the names chosen for your vector search endpoint, catalog and schema.![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf4a86caJeYqjysAcCD1fDZiFKj7fDL07AFXjf_vTizWD8tDRs9FVTwaN1LaotRAjAa-PTJJoWCp78dHIRF19myNF6hnmnWwbBX5e0pryU9BUnleQt9-GETwVwStsqIBt6aZj0BqWz-GjnUplMbh24?key=UNvoNqEwC0UYO-mNBhAdHukz)
