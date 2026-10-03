# Copyright 2024 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Define a default value so it's not empty if the builder fails to provide it
ARG BUILDPLATFORM=linux/amd64

FROM --platform=$BUILDPLATFORM python:3.15.0rc2-slim@sha256:14684656c0069b49e897c63d52bbbe7df8a4bf189911a597403fd3b4ffeaae06 AS base

FROM base AS builder

RUN apt-get -qq update \
    && apt-get install -y --no-install-recommends g++ \
    && rm -rf /var/lib/apt/lists/*

# get packages
COPY requirements.txt .
RUN pip install -r requirements.txt

FROM base

# Apply current Debian security updates
RUN apt-get update \
&& apt-get install -y --no-install-recommends \
libssl3t64 \
libpcre2-8-0 \
openssl \
openssl-provider-legacy \
&& rm -rf /var/lib/apt/lists/*

# Enable unbuffered logging
ENV PYTHONUNBUFFERED=1

# get packages
WORKDIR /shoppingassistantservice

# Grab packages from builder
COPY --from=builder /usr/local/lib/python3.14/ /usr/local/lib/python3.14/

#pip is only required during the build stage
RUN rm -rf /usr/local/lib/python3.14/site-packages/pip \
/usr/local/lib/python3.14/site-packages/pip-*.dist-info \
/usr/local/bin/pip \
/usr/local/bin/pip3 \
/usr/local/bin/pip3.14

#Create non-root user
RUN addgroup --system shoppingassistantservice \
&& adduser --system --ingroup shoppingassistantservice shoppingassistantservice

#Add the application with correct ownership
COPY --chown=shoppinassistantservice:shoppinassistantservice . .

USER shoppingassistantservice

# set listen port
ENV PORT="8080"
EXPOSE 8080

ENTRYPOINT ["python", "shoppingassistantservice.py"]
