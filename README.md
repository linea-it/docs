# LIneA Documentation

This repository is a fork of NERSC Documentation at [docs.nersc.gov](https://docs.nersc.gov). 

This repository contains LIneA [documentation](https://docs.linea.org.br/) written in Markdown which is converted to html/css/js with the [mkdocs](http://www.mkdocs.org) static site generator. The theme is [mkdocs-material](https://github.com/squidfunk/mkdocs-material) with LIneA customizations such as the colors injected via selective overloading of templates and CSS. The documentation pages are found in the top-level folder `docs`.

# Reporting Issues

To report a problem with our documentation please create an [issue](https://github.com/linea-it/docs/issues/new/choose) and someone will work on your task. Before you create a ticket, please check if there is an existing issue for the same problem to avoid duplicate issues.

If you need any further assistance send an email to `helpdesk@linea.org.br`.

# How to build the documentation

```
git clone https://github.com/linea-it/docs/
cd docs

apt-get install python-pip python3.10-venv
python3 -m venv mkdocs
source mkdocs/bin/activate
pip install -r requirements.txt
```

To run a local server using mkdocs:

```
cd docs
source mkdocs/bin/activate
mkdocs serve
```


# License

This repository is licensed under the BSD 3-Clause. For more details, see [LICENSE](https://github.com/linea-it/docs/blob/main/LICENSE).
