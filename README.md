# mBERT
**mBERT** is a mutation testing tool that uses [*CodeBERT*](https://github.com/microsoft/CodeBERT), a pre-trained language model, to generate mutants.  


## Requirements
- Maven
- [*CodeBERT*](https://github.com/microsoft/CodeBERT) dependencies:  
	- `pip install torch`
	- `pip install transformers`

## Installation
1. Run `mkdir -p pre-trained/codebert-base-mlm` to create the folder where *CodeBERT* pre-trained model will be saved.
2. Run `python3 download-codebert.py` to download *CodeBERT* pre-trained model.
3. Try *CodeBERT* by running: `python3 run-codebert.py "int <mask> = b;"`
> {'score': 0.23396340012550354, 'token': 740, 'token_str': 'c', 'sequence': 'int c= b;'}
> 
> {'score': 0.05450829118490219, 'token': 939, 'token_str': 'i', 'sequence': 'int i= b;'}
> 
> {'score': 0.05004948750138283, 'token': 741, 'token_str': 'b', 'sequence': 'int b= b;'}
> 
> {'score': 0.04164685681462288, 'token': 10, 'token_str': 'a', 'sequence': 'int a= b;'}
> 
> {'score': 0.023635799065232277, 'token': 181, 'token_str': 'p', 'sequence': 'int p= b;'}

4. Compile **mBERT**: `mvn compile`. 
5. Done! Try **mBERT** by running: `./mBERT.sh`

## Executing mBERT
**mBERT** provides some flags that you can configure:
- `-in=source_file_name` 
- `-out=mutants_directory`
- `-N=max_num_of_mutants`
- `-m=method_name`
- `-m=method_name:method_definition_line`
- `-l=line_to_mutate`

## Compile Mutants Generated

You can use script `compile-mutants.sh` to compile the mutants generated by **mBERT**.

- Usage: `./compile-mutants.sh mutants_dir subject_name` 
- Information: After compiling the mutants, you will find the details in files `mutants_dir/subject_name.csv` and `mutants_dir/subject_name.log` 
- Example: `./compile-mutants.sh examples/generated-mutants/gcd/ gcd`

## Examples
Forlder `examples` provide the examples discussed in the paper. 

Inside `examples/generated-mutants` you can find the mutants generated by **mBERT**.

## Run with docker

### build an image

`docker build -t mbert_image .`

### Run container

`docker run -it -v <local_project_directory>:<path to the container directory> -e TARGET_PROJECT_EXE_ROOT=<path> -e LOOKFOR_SOURCE_FILES=<path> mbert_image`

You need to sync local target project directory with application working dir
Next, you need to provide two params: 
- TARGET_PROJECT_EXE_ROOT: path to the target project root (where pom.xml is located, to execute mutants)
- LOOKFOR_SOURCE_FILES: path or a package to look for .java files and generate mutants

Example command:
`docker run -it -v /tmp/target_project:/usr/src/app/target_project -e TARGET_PROJECT_EXE_ROOT=target_project/commons-io -e LOOKFOR_SOURCE_FILES=src/main/java/org/apache/commons/io mbert_image`

Output: 
creates codebert_mutants dir with source code mutants, located under local_project_directory/..
creates killing_matrix.csv file where it outputs mutants IDs and failing tests


## Cite this paper
If you use **mBERT** in your research, please cite our paper:

	@inproceedings{DBLP:conf/icst/DegiovanniP22,
	  author    = {Renzo Degiovanni and
		       Mike Papadakis},
	  title     = {{\(\mathrm{\mu}\)}Bert: Mutation Testing using Pre-Trained Language
		       Models},
	  booktitle = {15th {IEEE} International Conference on Software Testing, Verification
		       and Validation Workshops {ICST} Workshops 2022, Valencia, Spain, April
		       4-13, 2022},
	  pages     = {160--169},
	  publisher = {{IEEE}},
	  year      = {2022},
	  url       = {https://doi.org/10.1109/ICSTW55395.2022.00039},
	  doi       = {10.1109/ICSTW55395.2022.00039},
	  timestamp = {Mon, 13 Jun 2022 16:53:37 +0200},
	  biburl    = {https://dblp.org/rec/conf/icst/DegiovanniP22.bib},
	  bibsource = {dblp computer science bibliography, https://dblp.org}
	}



