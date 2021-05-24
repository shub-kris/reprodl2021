FROM pytorch/pytorch:1.8.1-cuda11.1-cudnn8-devel


#Copy code and data
COPY . /reprodl
WORKDIR /reprodl

#Install all libraries
RUN apt-get update && apt-get upgrade -y
RUN pip install -r requirements.txt
RUN apt-get install -y libsndfile1-dev

#Run the training loop
CMD python train.py