# Flock.io - Validator guide:
# 1. Stake FML as Validator from train.flock.io
#In order begin receiving validation assignments, you must stake FML as to a task at train.flock.io.
#Navigate to the Validator tab on the Stake page
#
#Select task to validate
#
#Enter number of FML tokens to stake
#
#Stake FML
#
#Once you’ve confirmed and approved the transaction in your web3 wallet, you will see a box on the Validator tab with your stake details.
#2. Get API key
#Your API key is required for all remaining Validator steps. You can get it from the web app.
#Select dropdown in upper right corner of web app
#
#Select API
#
#On the API page, copy your API key
#
#Once you have your API key, you can proceed to the next step.
#3. Set up your environment
#To set up your environment, run the following commands:
#
#Windows
#For Windows users, we suggest installing WSL. Follow the guidance: WSL installation
#
#Mac/Linux
#You can install Anaconda via HERE
#
#4. Clone the Validator Repo
#The validator repo contains everything you need to run our script. To clone it run:
#
Copy
git clone https://github.com/FLock-io/llm-loss-validator.git
To set up all packages within the project directory

```
conda create -n llm-loss-validator python==3.10
conda activate llm-loss-validator
pip install -r requirements.txt
```
#5. Run Validation Script
#Now that you have staked as a Validator and have your API key you can run the validation script by running the following loop in your command line:
#
#Unix/Linux with Nvidia GPU
#Unix/Linux CPU only
#WSL
#PowerShell
```
cd /src
bash start.sh \
--hf_token your_hf_token \
--flock_api_key your_flock_api_key \
--task_id your_task_id \
--validation_args_file validation_config_cpu.json.example \
--auto_clean_cache False
```
#CUDA_VISIBLE_DEVICES=0: Specifies which GPU to use. 0 indicates the first GPU. Adjust this based on your available GPUs. Remove this line if you are using just the CPU
#
#--hf_token: Your Hugging Face token, required for accessing certain models. This should token should have write access.
#
#--flock_api_key: Your FLock API key.
#
#--task_id: The ID of the task you want to validate. If you are validating multiple tasks, you can pass a list eg. if you are validating tasks 8 and 9, you can pass --task_id 8,9
#
#--validation_args_file: The path to the validation arguments file.
#
#--auto_clean_cache: A flag to determine whether to automatically clean the model cache.
#
#--lora_only: A flag to indicate whether to validate only repositories with LoRA (Low-Rank Adaptation) weights. True means only LoRA weights will be validated. This is useful for validators with limited network bandwidth, as LoRA weights are significantly smaller (10-500 MiB) compared to full model files (>10 GiB)
#
#NOTE: If you would like to customise your validation script, visit this repo https://github.com/FLock-io/llm-loss-validator and check out How to run validation script section
#
#Make sure the HF_TOKEN you passed in have access to the gated base models like gemma, llama2, and llama3 to succesfully validate models fine-tuned based on those.
#
#Links:
#
#https://huggingface.co/google/gemma-7b
#
#https://huggingface.co/meta-llama/Meta-Llama-3-8B
#
#https://huggingface.co/meta-llama/Llama-2-7b-hf
#
#https://huggingface.co/google/gemma-2b
#
#6. Claim rewards
#Reward distribution is triggered every 24 hours at midnight UTC. You can claim your rewards via the web app.
#
#Once you’ve connected your web3 wallet to train.flock.io, you can claim rewards with the following steps:
#
#Navigate to the Validator tab on the Stake page
