# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# FROM python:3.12.6-slim@sha256:ad48727987b259854d52241fac3bc633574364867b8e20aec305e6e7f4028b26 AS base
FROM python:3.11.10-slim AS BASE
FROM base AS builder

RUN apt-get -qq update \
    && apt-get install -y --no-install-recommends \
        wget g++ \
    && rm -rf /var/lib/apt/lists/* 


# get packages
COPY requirements.txt .
RUN pip install --prefix="/reqs" -r requirements.txt



FROM base
# Enable unbuffered logging
ENV PYTHONUNBUFFERED=1
# Enable Profiler
ENV ENABLE_PROFILER=1

RUN apt-get update && apt-get upgrade && apt-get install -y curl


WORKDIR /email_server
COPY --from=builder /reqs /usr/local


# Grab packages from builder
# COPY --from=builder /usr/local/lib/python3.12/ /usr/local/lib/python3.12/
COPY --from=builder /usr/local/lib/python3.11/ /usr/local/lib/python3.11/

# Add the application
COPY . .

RUN opentelemetry-bootstrap -a install


EXPOSE 8080
EXPOSE 8000
# CMD ["opentelemetry-instrument", "python", "email_server.py"]
ENTRYPOINT ["opentelemetry-instrument", "python", "email_server.py" ]
