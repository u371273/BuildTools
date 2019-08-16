# vs2010-buildtools & Perl 5.26.1.1
This work is based on **vs2010-buildtools**:
```
Visit https://github.com/cryptlex/vs2010-buildtools
```

This provides a pre-build environment for projects created with Visual Studio 2010 and Perl.

Try it out with:
```
docker container run -it u371273/vs2010_perl5:1.0 powershell
```
If a local folder is to be exposed to the container, try with
```
docker container run -it -v $LocalFolder:C:\Users\MyData u371273/vs2010_perl5:1.0 powershell
```
where, $LocalFolder is the local path.
