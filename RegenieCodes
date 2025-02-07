# Running GWAS using Regneie

**Table of Contents**  
[1. QC](#1-quality-control-qc)  
[2. Regenie Step 1](#2-regenie-step-1)  
[3. Regenie Step 2](#3-regenie-step-2)  


<a name="1-quality-control-qc"></a>
## 1. Quality Control (QC)

```bash
# Paths (Update these)
PLINK2="/path/to/plink2"
Genotype="/path/to/TCGS_Chip"  # Base name without extension
OutputDir="/path/to/Regenie"
# Create output directory if it doesn't exist
mkdir -p ${OutputDir}


# Run QC
${PLINK2} \
  --bfile ${Genotype} \
  --geno 0.1 \       
  --hwe 1e-6 \          
  --maf 0.01 \          
  --mind 0.1 \         
  --make-bed \
  --out ${OutputDir}/TCGS_qc
```


<a name="#2-regenie-step-1"></a>
## 2. Regenie Step 1

```bash 
REGENIE="/path/to/regenie"
QC_DATA="${OutputDir}/TCGS_qc"
PHENO="/path/to/Obesity.txt"
COVAR="/path/to/Obesity_Covar.txt"

${REGENIE} \
  --step 1 \
  --bed ${QC_DATA} \
  --phenoFile ${PHENO} \
  --phenoCol BMI \              # Specify phenotype column
  --covarFile ${COVAR} \
  --covarColList age,PC1,PC2,PC3,PC4,PC5 \  
  --catCovarList sex \          # Specify categorical covariates
  --bsize 1000 \
  --lowmem \
  --threads 8 \
  --maxCatLevels 99 \
  --gz \
  --out ${OutputDir}/step1_results

```

<a name="#3-regenie-step-2"></a>
## 3. Regenie Step 2

```bash 
# path to Regenie step1 output
STEP1_PRED="${OutputDir}/step1_results_pred.list"

for CHR in {1..22} X; do
  BGEN="/path/to/imputed/chr${CHR}.bgen"
  SAMPLE="/path/to/imputed/chr${CHR}.sample"

  ${REGENIE} \
    --step 2 \
    --bgen ${BGEN} \
    --sample ${SAMPLE} \
    --phenoFile ${PHENO} \
    --phenoCol BMI \
    --covarFile ${COVAR} \
    --covarColList age,sex,PC1,PC2,PC3,PC4,PC5 \
    --catCovarList sex \
    --pred ${STEP1_PRED} \
    --bsize 400 \
    --minINFO 0.3 \
    --minMAC 2 \
    --threads 4 \
    --gz \
    --out ${OutputDir}/step2_chr${CHR}
done
```

 
