# ml-benchmarks

This is a project for benchmarking data-loaders, with an emphasis on over-the-network data loading.

# Set up

## Environmental variables

Create a `.env` file with the following information

```sh
DOCKER_NAME=<name of org>/<name of container>:<version>
DYNACONF_AWS_ACCESS_KEY_ID=<aws id>
DYNACONF_AWS_SECRET_ACCESS_KEY=<aws secret>
DYNACONF_BUCKET_NAME=<bucket name in aws> # needs to exist before running experiments
DYNACONF_S3_ENDPOINT=http://172.28.142.23:10000 # 
```

## Running locally

1. Clone this repository
2. Export the wheel password `export WHEEL_PASSWORD=<password>`
3. Decrypt the wheel: `./infrastructure/decrypt_secret.sh`
4. Build the docker container: `./scripts/build.sh`
5. Start the minio (S3 like) container: `./scripts/start_minio.sh` (check that IP and PORT match the `S3_ENDPOINT`)
6. Run the container: `./scripts/run.sh`
7. Run all the experiments: `./experiments/run_all.sh`

## Running on AWS

1. Create the file `~/.aws/credentials` with the following content:
```sh
[default]
aws_access_key_id = <aws id> 
aws_secret_access_key = <aws secret>
```
2. Make sure that an S3 bucket is created with the name defined above and that it is accessible with the credentials provided.
3. Download the `get_ecr` script to fetch the latest docker image: `wget https://raw.githubusercontent.com/kiedanski/dataloader-benchmarks/main/scripts/get_erc.sh && chmod +x get_ecr.sh`
4. Download the latest docker image locally: `./get_ecr.sh`
5. Download the run script: `wget https://raw.githubusercontent.com/kiedanski/dataloader-benchmarks/main/scripts/run.sh && chmod +x run.sh` 
6. Execute the run command to get into the docker container: `./run.sh`
7. Run all the experiments: `./experiments/run_all.sh`


## Collecting results and plotting

Inside the container run:

1. `python src/plots/download_results.py`
2. `python src/plots/generate_plots.py`



# Implemented Libraries and Datasets

|          |           | Pytorch | FFCV | Hub | Deep Lake | Torchdata | Webdataset | Squirrel |
| -------- | --------- | ------- | ---- | --- | --------- | --------- | ---------- | -------- |
| CIFAR-10 | default   | ✅      | ✅   | ✅  | ✅        | ✅        | ✅         |  ✅      |
|          | remote    | ❌      | ✅   | ✅  | ✅        | ❌        | ✅         |  ❓      |
|          | filtering | ✅      | ❓   | ✅  | ✅        | ✅        | ✅         |  ❓      |
|          | multi-gpu | ✅      | ✅   | ❌  | ✅        | ✅        | ✅         |  ✅      |
| RANDOM   | default   | ✅      | ✅   | ✅  | ✅        | ✅        | ✅         |  ✅      |
|          | remote    | ❌      | ✅   | ✅  | ✅        | ❌        | ✅         |  ❓      |
|          | filtering | ✅      | ❓   | ✅  | ✅        | ✅        | ✅         |  ❓      |
|          | multi-gpu | ✅      | ✅   | ❌  | ✅        | ✅        | ✅         |  ✅      |
| CoCo     | default   | ✅      | ❌   | ✅  | ✅        | ✅        | ✅         |  ✅      |
|          | remote    | ❌      | ❌   | ✅  | ✅        | ❌        | ✅         |  ❓      |
|          | filtering | ✅      | ❌   | ✅  | ✅        | ✅        | ✅         |  ❓      |
|          | multi-gpu | ✅      | ❌   | ❌  | ✅        | ✅        | ✅         |  ✅      |
