FROM python:3.8-buster
LABEL maintainer="khadija<khadija.ibourk.2003@gmail.com>"
RUN apt update -y && apt install -y python3-dev libsasl2-dev libldap2-dev libssl-dev
COPY requirements.txt /
RUN pip3 install -r /requirements.txt
COPY student_age.py /
RUN mkdir /data
VOLUME /data
EXPOSE 5000
CMD ["python3", "./student_age.py"]