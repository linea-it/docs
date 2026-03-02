# Apptainer

## O que é?
O **[Apptainer](https://apptainer.org/docs/user/1.4/introduction.html)** é uma plataforma de containers amplamente utilizada em ambientes científicos e de HPC. Ele foi projetado para permitir a execução de aplicações complexas em clusters de forma **segura, portátil e reproduzível**, garantindo que o ambiente de execução seja idêntico independentemente do nó onde o job será executado.

Em ambientes multiusuário, como clusters, o Apptainer se destaca por funcionar sem a necessidade de privilégios administrativos, integrando-se naturalmente a gerenciadores de fila como o Slurm.

O Apptainer utiliza imagens no formato **SIF (_Singularity Image Format_)**, que consistem em um **arquivo único e imutável** contendo todo o sistema necessário para executar a aplicação, incluindo sistema operacional, bibliotecas, dependências e softwares adicionais. Isso facilita o compartilhamento, a versionamento e a reprodutibilidade dos experimentos.

## Como utilizar o Apptainer - Cluster Apollo
No **Cluster Apollo**, o uso do Apptainer é permitido apenas para **execução de imagens já construídas**.

Por motivos de segurança e padronização do ambiente:
!!! WARNING "Atenção:"
    **Não é permitido realizar build de imagens (`apptainer build`) nos nós do cluster.**

As imagens devem ser construídas externamente (em máquina local ou outro ambiente apropriado) e enviadas ao cluster no formato `.sif`.

### Passo a passo de Uso
1. **Upload da imagem `.sif`**

    O usuário deve:
    
       - Acessar a plataforma **[LIneA Open OnDemand](https://ondemand.linea.org.br)**
       - Realizar o upload do arquivo `.sif` em seu diretório de **scratch**
             <br>caminho: `/scratch/YOUR.USER/IMAGE_NAME.sif`</br> 

2. **Escolher a forma de execução**
 
O usuário pode executar a aplicação de duas maneiras principais:

   - **Opção A** – Submissão via Slurm (Forma Recomendada):

Criar um script de submissão, por exemplo `job_apptainer.sh`:
```Shell
#!/bin/bash
#SBATCH -p <PARTITION>                       
#SBATCH --nodes=<QT-NODE>
#SBATCH --account=<ACCOUNT-NAME>	           
#SBATCH -J <JOB-NAME>           
#----------------------------------------------------------------------------#
echo "SLURM_NODELIST=$SLURM_NODELIST"

# Caminho da imagem
IMAGE=/scratch/users/YOUR.USER/IMAGE_NAME.sif

# Executa comando dentro do container
srun apptainer run --containall $IMAGE

echo "Finalizado"
```


   - **Opção B** – Alocação Interativa:

Para testes ou execução interativa:

  1. Faça alocação de um nó de computação:
```Shell        
salloc -p PARTITION-NAME --nodelist=NODE
ssh NODE
```

  2. Após alocar o nó e acessá-lo via SSH, execute:
```Shell
apptainer exec /scratch/YOUR.USER/IMAGE_NAME.sif YOUR_CODE
```

  3. Ou ainda, se desejar entrar no ambiente do container:
```Shell
apptainer shell /scratch/YOUR.USER/IMAGE_NAME.sif
```
Isso abrirá um terminal dentro da imagem. Dentro do container, o usuário pode executar comandos normalmente.

## Boas Práticas no Cluster - Apptainer

✔ Sempre execute via Slurm  
✔ Utilize o `scratch` para os arquivos de imagens e dados temporários  
✔ Teste interativamente antes de submeter jobs longos  
✔ Mantenha suas imagens versionadas

Qualquer dúvida, entre em contato com o [Service Desk](http://localhost:8088/suporte.html).
