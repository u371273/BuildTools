# This docker image is going to be huge! :'(

FROM mcr.microsoft.com/dotnet/framework/sdk:3.5

# install chocolatey
RUN powershell -Command Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# RUN powershell -Command choco install -y DotNet4.0 7zip windows-sdk-7.1
RUN powershell -Command choco install -y windows-sdk-7.1

# Unfortunately, the choco packages don't really work for our needs so we have
# to install a bunch of MSIs manually. Fun times!
COPY *.ps1 /scripts/

# Install the vs 2010 content from DVD
RUN powershell -Command /scripts/install_tools.ps1 -InstallURI http://download.microsoft.com/download/F/1/0/F10113F5-B750-4969-A255-274341AC6BCE/GRMSDK_EN_DVD.iso

RUN REG ADD 'HKLM\SOFTWARE\Microsoft\Microsoft SDKs\Windows\v7.0A' /f /v InstallationFolder /t reg_sz /d 'C:\\Program Files\\Microsoft SDKs\\Windows\\v7.1\\'

# Needed for resource files
RUN powershell -Command choco install -y windows-sdk-8.0

# Needed for setting up the .NET environment
COPY *.bat /scripts/

ENV CMKDIR  'C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin'

RUN powershell Copy-Item C:\scripts\vcvars32.bat '$env:CMKDIR'

# The following steps are for installing Perl
ENV PERL_VERSION 5.26.1.1
ENV PERL_SHA256 1bb54d7f5e487ff468bf980dbfd9962fb9d83ff1f10ccde008e0bee1f1f5b6ce

RUN powershell -Command $ErrorActionPreference = 'Stop' ; $ProgressPreference = 'SilentlyContinue' ; \
    Invoke-WebRequest $('http://strawberryperl.com/download/{0}/strawberry-perl-{0}-64bit-portable.zip' -f $env:PERL_VERSION) -OutFile 'perl.zip' -UseBasicParsing ; \
    if ((Get-FileHash perl.zip -Algorithm sha256).Hash -ne $env:PERL_SHA256) { Write-Error 'SHA256 mismatch' } ; \
    Expand-Archive -Path perl.zip -DestinationPath C:\ -Force ; \
    rm perl.zip ; \
    setx /M PATH $('C:\perl\site\bin;C:\perl\bin;C:\c\bin;{0}' -f $env:PATH)





