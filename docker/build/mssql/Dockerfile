# escape=`

ARG BASE_IMAGE
ARG HRZ_IMAGE
ARG HEADLESS_IMAGE
ARG SITECRON_IMAGE

FROM ${HRZ_IMAGE} as horizon
FROM ${HEADLESS_IMAGE} as headless
FROM ${SITECRON_IMAGE} as sitecron
FROM ${BASE_IMAGE}

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Add Horizon Module
COPY --from=horizon \module\db \horizon_data
RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\horizon_data; `
    Remove-Item -Path C:\horizon_data -Recurse -Force;

# Add Headless Module
COPY --from=headless c:\module\db c:\jss_data
RUN C:\DeployDatabases.ps1 -ResourcesDirectory c:\jss_data; `
    Remove-Item -Path C:\jss_Data -Recurse -Force;

# Add Sitecron Module
COPY --from=sitecron \module\db \sitecron_data
RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\sitecron_data; `
    Remove-Item -Path C:\sitecron_data -Recurse -Force;