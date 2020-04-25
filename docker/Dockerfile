FROM continuumio/miniconda3:4.7.10

RUN adduser --disabled-password --gecos "Default user" --uid 1000 cq && \
    apt-get update -y && \
    apt-get install --no-install-recommends -y libgl1-mesa-glx libglu1-mesa && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir /data && chown cq:cq /data

ADD --chown=cq:cq examples /home/cq

RUN chown -R 1000 /home/cq

COPY environment.yml labextensions.txt /tmp/
RUN conda env create -f /tmp/environment.yml -n cq && \
    conda run -n cq conda install -y nodejs
RUN conda run -n cq jupyter labextension install --no-build $(cat /tmp/labextensions.txt) && \
    conda run -n cq jupyter lab build --dev-build=True --minimize=False

VOLUME /home/cq/
WORKDIR /home/cq
EXPOSE 8888
USER root
USER cq

COPY jupyter_cadquery/cadquery/replay.py /opt/conda/envs/cq/lib/python3.7/site-packages/jupyter_cadquery/cadquery/replay.py
COPY jupyter_cadquery/cad_view.py /opt/conda/envs/cq/lib/python3.7/site-packages/jupyter_cadquery/cad_view.py
CMD bash -l -c "source /opt/conda/bin/activate cq && jupyter lab --ip=0.0.0.0 --no-browser --NotebookApp.token='' --NotebookApp.allow_origin='*'"
