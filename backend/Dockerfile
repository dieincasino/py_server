FROM python:3.12-alpine

WORKDIR /app

COPY app.py .

RUN adduser -D -u 1000 user
USER user

EXPOSE 8080

CMD ["python3", "app.py"]

