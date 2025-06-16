FROM python:3.8-slim

# Build with 'docker build -t rife -f ./docker/Dockerfile .'
# Start with 'docker run --gpus all --rm -it -v $(pwd):/host rife'
# Run with for instance 'inference_video --video input.mp4'

# install deps
RUN apt-get update && apt-get -y install \
    bash ffmpeg unzip

# setup RIFE
WORKDIR /rife
COPY . .

ENV \
    PIP_ROOT_USER_ACTION=ignore \
    PIP_BREAK_SYSTEM_PACKAGES=1

RUN pip3 install --no-cache-dir -r requirements.txt gdown

ADD docker/inference_img /usr/local/bin/inference_img
RUN chmod +x /usr/local/bin/inference_img
ADD docker/inference_video /usr/local/bin/inference_video
RUN chmod +x /usr/local/bin/inference_video

# add pre-trained models
# Download file by id from google drive to tmp and extract to /rife/train_log/
# Remove zip file after extract to reduce container image size
RUN gdown --output /tmp/RIFE_trained_model.zip 1APIzVeI-4ZZCEuIRE1m6WYfSCaOsi_7_ && \
    unzip "/tmp/RIFE_trained_model.zip" && \
    rm /tmp/RIFE_trained_model.zip

WORKDIR /host
ENTRYPOINT ["/bin/bash"]

ENV NVIDIA_DRIVER_CAPABILITIES all