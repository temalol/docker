FROM python:3.5

COPY ./app .

RUN pip install -r requirements.txt


ENV FLASK_CONFIG=development
ENV FLASK_APP=start.py

ENTRYPOINT ./wait-for-it.sh $POSTGRES_HOST:5432 && flask run --host=0.0.0.0 --port=5000