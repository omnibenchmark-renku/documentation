# Step-by-step guide

This short guide will show you how to add a method's wrapper to Omnibenchmark. We will **add a classification method to our example Iris omnibenchmark** (a simple benchmark using the Iris dataset). 

## **1. Create a new project** 

Each of an Omnibenchmark is basically a **Renku project** (a Gitlab project with many tweaks). To add a new method to the Iris Omnibenchmark, we will create a new Renku project to host our code. Luckily, you don't have to code a whole new Renku project from scratch! You will see how to **add pre-filled project to any Omnibenchmark**: 

<iframe src="https://scribehow.com/embed/Create_an_Iris_method_project__wZy31A3lTpCGudZRot-ejA?skipIntro=True" width="100%" height="640" allowfullscreen frameborder="0"></iframe>



!!! note
    **Congrats, you just created your first project on Renku using our dedicated _Omnibenchmark templates_ (pre-filled projects).** The projects also come with their own set of instructions (in the README) but you can ignore these for now. The next sessions will cover them. 

## **2. Set up a working environment**

We have created a new project to host our code. One easy way to work on a project is to start an **interactive session** to work interactively on our project. Let's see how to set this up: 

<iframe src="https://scribehow.com/embed/Set_up_a_working_environment__kstdZbfyRyyMCYkdrDq4jg?skipIntro=true" width="100%" height="640" allowfullscreen frameborder="0"></iframe>

## **3. Add some code to your project**

We can now **modify, run and save our work** from an interactive session. Let's see how to add code to work with the Iris dataset; 

### **A. Set up the metadata for the project**

Open **`src/config.yaml`**. This file tells Omnibenchmark how to run the project and the metadata associated to it. Modify the file as follows (highlighted lines): 

=== "src/config.yaml" 
    ```{ .yaml .annotate hl_lines="10 11 12 13 18 19 22 23"}
    ---
    data:
        name: "iris-method-bc2-[YOUR_NAME]"
        title: "iris method example BC2"
        description: ""
        keywords: ["iris_method_bc2"]
    script: "src/iris-method-bc2-anthonysonrel1ecd.R"
    benchmark_name: "iris_example"
    inputs:
        keywords: ["iris_dataset"] ## Keyword(s) for the input dataset to import. 
        files: ["input"]  ## Input file type(s).
        prefix:
            input: "_dataset"
    outputs:
        template: "data/${name}/${name}_${unique_values}_${out_name}.${out_end}" ## Automatic. To change if you want specific output names. 
        files:
        ## File patterns to use for automatic detection. 
            method_result1: 
                end: "rds"
    parameter:
        ## Section to describe the parameter dataset, values and filter. 
        keywords: ["iris_parameters"] ## Keyword(s) of the parameters project(s) to import.
        names: ["rseed"] ## Name(s) of the parameter(s) to use from the parameter project that is imported.

    ```


!!! info
    The `src/config.py` is ported with **all** omnibenchmark projects. It is always pre-filled for you and is usually the first file that you have to modify.


### **B. Add code to the project**

Open your R script: **`src/iris-method-bc2-[...].R`**. Copy-paste the following code in your script (we'll go through it together); 

=== "src/iris-method-bc2[...].R"
    ```{ .R}
    # Load package
    library(optparse)

    # Get list with command line arguments by name
    option_list = list(
        make_option(c("--input"), type="character", default=NULL, 
                help="Description of the argument", metavar="character"), 
        make_option(c("--rseed"), type="character", default=NULL, 
                    help="Description of the argument", metavar="character"), 
        make_option(c("--method_result1"), type="character", default=NULL, 
                    help="Description of the argument", metavar="character")
    ); 
    
    opt_parser = OptionParser(option_list=option_list);
    opt = parse_args(opt_parser);


    # Call the arguments
    input <- opt$input
    rseed <- opt$rseed
    method_result1 <- opt$method_result1

    # Call the method
    library(caret)
    dat <- read.csv(input)

    dat$Species <- as.factor(dat$Species)

    # Split
    validation_index <- createDataPartition(dat$Species, p=.5, list=FALSE)
    validation <- dat[-validation_index,]
    dat_train <- dat[validation_index,]

    set.seed(rseed)
    control <- trainControl(method="cv", number=10)
    metric <- "Accuracy"
    fit.lda <- train(Species~., data=dat_train, method="lda", metric=metric, trControl=control)

    print(fit.lda)
    saveRDS(object = fit.lda, file = method_result1)

    ```


### **C. Run the workflow**

We have specified how the project should run and the code associated to it. 

We can now run the workflow by running the **`src/run_workflow.py`**. 

Open a **new Python console** from the base directory. We will go through it together.

!!! info
    The `src/run_workflow.py` is ported with **all** omnibenchmark projects. Its only purpose is to run your code and **doesn't need to be modified**. 
