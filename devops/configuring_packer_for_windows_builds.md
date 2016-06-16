# Configuring Packer for Windows build

## Configuring the autounattend.xml file

### Prequisites

1. DISM ( Windows )

  Deployment Image Servicing and Management (DISM.exe) is a command-line tool to examining Windows images. 
  Installed as part of the Windows Assessment and Deployment Kit

  ```shell
  choco install windows-adk
  ```

2. Windows installation media in ISO format

  Note that Windows ISO's created using the [Media Creation Tool][1] are not compatible with the ```DISM``` tool.
  Instead - obtain the installation media from the Microsoft Tech Bench web site. 

  [1]: https://www.microsoft.com/en-gb/software-download/techbench "Microsoft Tech Bench"

### Extract Windows image metadata required by Packer config

This is required by the ```<ImageIstall><OSImage><InstallFrom><MetaData>``` element of the ```autounattend.xml``` file.

Mount the iso associated with the Windows image and locate the *Windows Imaging Format* ( ```.wim``` file ) that contains the image metadata.
For Windows 10 this appears to be the ```install.wim``` file in the root level ```sources``` directory within the mounted iso.

Examine the ```wim``` file using the ```DSIM``` tool as follows:

```shell
dism /Get-WinInfo /WimFile:install.vim
```

This will yield the image names contained in the iso file, for example:

```shell
Index : 1
Name : Windows 10 Pro
Description : ...
Size : ...
```

### Time-zone information

Use the following command from a Windows box to list timezone strings:

```shell
tzutil /l
```

To find the currently installed timezone use:

```shell
tzutil /g
```

This gives the following configuration in the ```Autounattend.xml``` file to install UTC:

```shell
<TimeZone>UTC</TimeZone>
```





