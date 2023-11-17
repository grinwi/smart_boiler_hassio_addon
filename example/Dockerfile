# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

# Execute during the build of the image
ARG TEMPIO_VERSION BUILD_ARCH
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}"

# Copy root filesystem
COPY rootfs /

FROM python
WORKDIR /app
COPY src/smart_boiler/requirements.txt /app/requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# copying of files needed to run the script (modules etc...)
COPY src/smart_boiler/control.py /app/control.py
COPY src/smart_boiler/boiler.py /app/boiler.py
COPY src/smart_boiler/time_handler.py /app/time_handler.py
COPY src/smart_boiler/week_planner.py /app/week_planner.py
COPY src/smart_boiler/event_checker.py /app/event_checker.py
COPY src/smart_boiler/settings.json /app/settings.json

CMD ["python", "-u", "control.py","-f", "settings.json"]

