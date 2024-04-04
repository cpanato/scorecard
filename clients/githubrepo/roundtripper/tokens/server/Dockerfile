# Copyright 2021 OpenSSF Scorecard Authors
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

FROM golang@sha256:ea3d912d500b1ae0a691b2e53eb8a6345b579d42d7e6a64acca83d274b949740 AS base
WORKDIR /src
ENV CGO_ENABLED=0
COPY go.* ./
RUN go mod download
COPY . ./

FROM base AS authserver
ARG TARGETOS
ARG TARGETARCH
RUN CGO_ENABLED=0 make build-github-server

FROM gcr.io/distroless/base:nonroot@sha256:0ba510e7d2cd07b3481c6760b2f571724b337d56a6788b9d28d62afa3a0d9371
COPY --from=authserver /src/clients/githubrepo/roundtripper/tokens/server/github-auth-server clients/githubrepo/roundtripper/tokens/server/github-auth-server
ENTRYPOINT ["clients/githubrepo/roundtripper/tokens/server/github-auth-server"]
