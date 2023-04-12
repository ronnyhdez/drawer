# Docker

Notes about working with Docker

## Steps to run container with mounted volumes

 - I have a model that takes as an input an image
 - A model make predictions over the image
 - The output is an xml file
 - I need the container to read the image from an specific directory in the host
 - I need the container to write the results in an specific directory in the hots

The Dockerfile:

```
FROM ubuntu:latest

# Update package list and install GDAL and Python dependencies
RUN apt-get update && \
    apt-get install -y python3 python3-pip gdal-bin libgdal-dev ffmpeg libsm6 libxext6

# Set environment variables for GDAL
ENV LD_LIBRARY_PATH=/usr/lib
ENV GDAL_DATA=/usr/share/gdal/

EXPOSE 8080

# Keeps Python from generating .pyc files in the container
ENV PYTHONDONTWRITEBYTECODE=1

# Turns off buffering for easier container logging
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Install pip requirements
COPY ./requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir --upgrade -r /app/requirements.txt

COPY . /app

# Define the input and output folder as volumes
VOLUME /app/input_files
VOLUME /app/predictions

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-python-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

CMD ["python3", "predict_process.py", "--input_folder=/app/input_files", "--output_folder=/app/predictions"]
```

After I build the image with the name predict_io, I can run the container
indicating the paths in my host. Also, this container needs to run using
the host GPU:

```
docker run --gpus all -v /home/ronny/Desktop/io_test/in/:/app/input_files -v /home/ronny/Desktop/io_test/out/:/app/predictions predict_io
```

When the container runs, it will retrieve the image from 
**/home/ronny/Desktop/io_test/in/**, and it will write the results on
**/home/ronny/Desktop/io_test/out/**

If I receive the error: `Permission denied`, I will have to change the
permissions in the host output folder. I did it with:

```
chmod a+w /home/ronny/Desktop/io_test/out/
```
