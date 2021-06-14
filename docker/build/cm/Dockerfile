# escape=`

ARG BASE_IMAGE
ARG TOOLING_IMAGE
ARG HRZ_IMAGE
ARG HEADLESS_IMAGE
ARG SITECRON_IMAGE
#ARG SOLUTION_IMAGE

#FROM ${SOLUTION_IMAGE} as solution
FROM ${TOOLING_IMAGE} as tooling
FROM ${HRZ_IMAGE} as horizon
FROM ${HEADLESS_IMAGE} as headless
FROM ${SITECRON_IMAGE} as sitecron
FROM ${BASE_IMAGE}

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Copy development tools and entrypoint
COPY --from=tooling \tools\ \tools\

WORKDIR C:\inetpub\wwwroot

# Add Headless module
COPY --from=headless \module\cm\content .\
COPY --from=headless \module\tools \module\tools
RUN C:\module\tools\Initialize-Content.ps1 -TargetPath .\; `
    Remove-Item -Path C:\module -Recurse -Force;

# Add Horizon Module
COPY --from=horizon \module\cm\content .\

# Add Sitecron Module
COPY --from=sitecron \module\cm\content .\

# Copy solution website files
#COPY --from=solution /artifacts/platform/ ./