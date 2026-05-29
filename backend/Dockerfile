FROM python:alpine3.12

WORKDIR /app

COPY app.py .

RUN adduser -D -u 1000 user

USER user

EXPOSE 8080

CMD ["python3", "app.py"]


