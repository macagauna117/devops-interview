FROM python:3.7-slim-buster

# The enviroment variable ensures that the python output is set straight
# to the terminal with out buffering it first
ENV PYTHONUNBUFFERED=1

WORKDIR /back

COPY requirements.txt /back/

RUN apt-get update && apt-get -y install python3-pip
RUN apt-get -y install graphviz-dev 

RUN pip3 install -r requirements.txt &&  apt autoremove -y 
RUN pip3 install pygraphviz 

#copia los archivos de la app
COPY . /back/

EXPOSE 8000

CMD [ "python", "manage.py", "runserver",  "0.0.0.0:8000" ]


