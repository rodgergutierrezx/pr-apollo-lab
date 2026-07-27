FROM condaforge/miniforge3:latest

ARG CONDA_ENV=ai

ENV CONDA_ENV=${CONDA_ENV}
ENV PATH=/opt/conda/envs/${CONDA_ENV}/bin:/opt/conda/bin:$PATH
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PIP_NO_CACHE_DIR=1

WORKDIR /workspace

COPY docker/cpu/environment.yml /tmp/environment.yml

RUN conda env create -f /tmp/environment.yml \
    && conda clean -afy \
    && rm /tmp/environment.yml

RUN conda run -n ${CONDA_ENV} \
    python -m ipykernel install \
    --prefix=/opt/conda \
    --name=${CONDA_ENV} \
    --display-name="Python (AI CPU)"

EXPOSE 8888

CMD ["conda", "run", "--no-capture-output", "-n", "ai", \
     "jupyter", "lab", \
     "--ip=0.0.0.0", \
     "--port=8888", \
     "--no-browser", \
     "--allow-root", \
     "--ServerApp.root_dir=/workspace"]
