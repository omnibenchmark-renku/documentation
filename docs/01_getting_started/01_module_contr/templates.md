
## Templates

Any part of Omnibenchmark can be extended with an additional project that will add a new dataset, method or metric to the evaluation. 

To ease the addition of a new project to Omnibenchmark, you can select a **template** when creating a new Renku project. 

!!! note

    Omnibenchmark is built upon the Renku system but the templates are made in a way that you won't have to worry about the underlying Renku mechanisms! You will just have to add your data and/or your code to project that will extend Omnibenchmark. 


Using templates on Renku
-------------------------

The following steps will show you how to create a new Omnibenchmark project for **any step** (data, method, metric,...).

1. On `Renku <https://renkulab.io/>`_ start a new project by clicking the "+" symbol and add a tittle and a description to your project (this will appear under your project title in your list of projects). 

![image](https://raw.githubusercontent.com/omnibenchmark/documentation/master/docs/images/datatemplate0.png)

2. On the "New project" page, fill in the required information. Under "Repository URL", indicate https://github.com/omnibenchmark/contributed-project-templates

3. Under "Repository Reference", indicate "main".

4. Fetch the templates. 

![image](https://raw.githubusercontent.com/omnibenchmark/documentation/master/docs/images/datatemplate1.png)

5. Once completed, select the desired omnibenchmark template (here as example, the "Basic omnibenchmark dataset" template to add a new dataset) and add all metadata that are required (you will be able to modify them latter if needed).

6. Create your project.

![image](https://raw.githubusercontent.com/omnibenchmark/documentation/master/docs/images/datatemplate2.png)

7. The template will generate a new project will all the required tools for you to extend omnibenchmark. You will find further instructions in the dedicated `Module documentation <https://omnibenchmark.readthedocs.io/en/latest/start/02_omnibenchmark_modules.html>`_ , which will depend on the template that you selected. 



