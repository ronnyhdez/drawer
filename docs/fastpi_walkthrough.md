# How to create an API with FastAPI and Docker

First, create repo that will contain all the files. Then open the repo
from VScode. 

Create a new python file. In the python file, we can create a simple API like
the following:

```
import uvicorn
from fastapi import FastAPI

app = FastAPI()

inventory = {
    1: {
        "name": "Milk",
        "price": 3.99,
        "brand": "dospinos"
    },
    2: {
        "name": "Milk",
        "price": 4.99,
        "brand": "coronado"
    }
}


@app.get("/get-item/{item_id}")
def get_item(item_id: int):
    return inventory[item_id]


# if __name__ == '__main__':
#     uvicorn.run(app, host='0.0.0.0', port=8000)
```

Once the file is created, before running the file, I have to create the
environment:

```
pipenv install
```

Then, let's activate the environment

```
pipenv shell
```

Let's check the environment

```
pipenv --venv
```

Everything setup, with the last lines of code in the python file with the API
we can run it with:

```
python3 name_of_file.py
```

That will serve the API in port 8000. We can go and check if it's running
on localhost:8000

If, those lines are not present, we can use the `uvicorn` package to serve the
API:

```
uvicorn api_example_1:app
```

Note that there is no `.py` in the name of the file we want to run.

## Docker

Now that we have build the API and were able to serve it, we can dockerized.

For this, with the Docker extension in VScode, we can press `Fn + F1` to
create a file. If the extension throws an error about no connection to daemon
socket, we need to aggregate Docker to sudo permissions:

```
sudo usermod -aG docker ${USER}
```

I had to restart computer to make this work. After that I was able to use the
Docker extension. After opening the options, I look for *add docker file*
and wrote the options asked.

After this, a Docker file was created and also the `requirements.txt` file
to be used during the image build process with venv (I think).

The Docker file looks like the following:

```
# For more information, please refer to https://aka.ms/vscode-docker-python
FROM python:3.10-slim

EXPOSE 8090

# Keeps Python from generating .pyc files in the container
ENV PYTHONDONTWRITEBYTECODE=1

# Turns off buffering for easier container logging
ENV PYTHONUNBUFFERED=1

# Install pip requirements
COPY requirements.txt .
RUN python -m pip install -r requirements.txt

WORKDIR /app
COPY . /app

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-python-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

# During debugging, this entry point will be overridden. For more information, please refer to https://aka.ms/vscode-docker-python-debug
CMD ["gunicorn", "--bind", "0.0.0.0:8090", "-k", "uvicorn.workers.UvicornWorker", "api_example_1:app"]
```

Now, the next step is to build the image and run it:

```
docker built -t test-api .
```

Then, run the container:

```
docker run -p 8000:8090 test-api
```

The instruction will bind the port 8000 on the host with the port 8090 in the
container.

Now we can check the container service in localhost:8000


