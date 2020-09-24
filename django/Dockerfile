FROM python:3.7
MAINTAINER whb whbjzzwjxq@gmail.com
EXPOSE 8000
COPY ../../requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
COPY .. .
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]