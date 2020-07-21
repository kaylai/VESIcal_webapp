# ENKI-Cloud-App: Fe-Ti-Oxide-Geotherm

This project demonstrates an App built with the [ENKI-portal ThermoEngine package](https://gitlab.com/ENKI-portal/ThermoEngine) that implements the Fe-Ti oxide geothermooxybarometer of [Ghiorso and Evans (2008)](https://gitlab.com/ENKI-portal/app-fe-ti-oxide-geotherm/AJS_2008_Ghiorso_Evans.pdf). The App is deployed as a containerized [Docker](https://www.docker.com) image on [Google Cloud](https://cloud.google.com) using the Binder service [MyBinder](https://mybinder.org).  It has a user interface built with [Jupyter Widgets](https://ipywidgets.readthedocs.io/en/latest/) and uses the Python package [appmode](https://github.com/oschuett/appmode) to hide the code interface from the user when launched.

Launch this app using the Binder launch badge on the repository interface. Please cite the results from this app using the DOI badge indicated.

In addition to providing code that demonstrates a functional user interface for Fe-Ti oxide geothermooxybarometry, this repository is a template for App or Jupyter notebook distribution of calculators and services built upon the [ThermoEngine package](https://gitlab.com/ENKI-portal/ThermoEngine) of the [ENKI project](http://enki-portal.org).

---

## Workflow for App development
To use this repository as a template for builing your own application, follow this workflow:
- Clone or fork the repository to your own user space on GitLab.com (or equivalent)
- Modify the repository Dockerfile as required. A Dockerfile is used to build a containerized software environment that supports executaion of your App. The container image can be run using Docker either on your desktop system or in the cloud. The current Dockerfile file,
```
FROM registry.gitlab.com/enki-portal/thermoengine:master
COPY . ${HOME}
USER root
RUN chown -R ${NB_UID} ${HOME}
RUN pip install --no-cache-dir appmode
RUN jupyter nbextension enable --py --sys-prefix appmode
RUN jupyter serverextension enable --py --sys-prefix appmode
USER ${NB_USER}
```
references a base container image of the ENKI software ecosystem upon which the App is constructed. That image includes the [ThermoEngine package](https://gitlab.com/ENKI-portal/ThermoEngine), [C and C++ compilers](http://clang.llvm.org), an [Ubuntu](https://ubuntu.com) Linux system, and a full [Jupyter](https://jupyter.org) enabled development environment under [Python 3.7](https://www.python.org). The Docker file additionally installs [*appmode*](https://github.com/oschuett/appmode) and links it to the Jupyter components. Note that additional Python packages required for your App should be specified for installation in this Dockerfile using Docker *RUN* commands that invoke *pip install*.
- Replace the Jupyter notebook included with this package (e.g., *Oxide-Geothermometer.ipynb*) with one (or more) that implement your App. You can develop your App as a Jupyter notebook on the ENKI server, or on your Desktop/Laptop.  For the later, install Docker on your local computer (there are versions for MacOS, Windows and Linux) and execute these two commands in a terminal window:
```
docker login registry.gitlab.com
docker run -p 8888:8888 --env JUPYTER_ENABLE_LAB=yes --user root -e GRANT_SUDO=yes registry.gitlab.com/enki-portal/thermoengine:master start-notebook.sh
```
These comands will start the image and provide a browser link to access the containerized ENKI software ecosystem for development on your local machine.
- When you are ready to distribute your App, visit [MyBinder](https://mybinder.org) and create a link to your repository and App notebook(s). You can publish that link to a website, include it in a paper or create a badge for your repository as is done here. As an example, this repository uses the following link for App execution: 
```
https://mybinder.org/v2/gl/ENKI-portal%2Fapp-fe-ti-oxide-geotherm/master?urlpath=apps%2FOxide-Geothermometer.ipynb
```
- When a user clicks on your link, MyBinder examines your repository and checks to see if it is new or has recently changed. If so, it runs Docker and builds your image container; otherwise it uses a previously stored image. After image retrieval, MyBinder allocates and lauches a cluster node in Google Cloud, installs your image on that node, and starts up the App. Users interact with your App via this free cloud service.  MyBinder provides this service courtesy of funding from Google cloud and the [Arthur P. Sloan Foundation](https://sloan.org).

---

## Citing your App in research publications
If you have gone through all the trouble to provide an App that generates results that users may want to cite in research applications, you should provide a repository DOI to get credit for your work. For this App we use the [Zenodo](https://zenodo.org) service to generate a repository DOI. You can do the same, and feel free to register your DOI with the ENKI-portal group on Zenodo. 