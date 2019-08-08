---
layout: post
title: Snakemake gotchas related to checkpoints
tags: snakemake how-to gotcha checkpoint
---

# Based on personal experiences...

The following gotchas were observed using Snakemake 5.5.0

## Checkpoints leading to a (silent) ambiguous rule exception

Based on the design pattern as specified in the Snakemake docs
(i.e., [checkpoint] split -> [rule] process -> [rule] merge),
putting the `split` and `process` output into the same folder
leads to an ambiguity that seems not to be properly handled
(discovered via `pdb`). Consider the following:

```
checkpoint split:
    input: <something>
    output:
        directory(output_path)

rule intermediate_process:
    input:
        output_path/split_file.foo
    output:
        output_path/split_file.bar
```

The `checkpoint` output matches also the output of the `intermediate_process`
rule, which creates the ambiguity. As usual, create more and more subfolders
to avoid this problem...

## Checkpoint re-evaluation with existing output fails

Same setting as above, so this one could be confirmed using
the example from the Snakemake docs...

```
checkpoint split:
    input: <something>
    output:
        directory(output_path/subfolder)

rule intermediate_process:
    input:
        output_path/subfolder/split_file.foo
    output:
        output_path/split_file.bar

def collect_split_files(wildcards):
    pass

rule merge:
    input:
        collect_split_files
    output:
        result_path/merged_splits.tsv
```

Deleting the output `result_path/merged_splits.tsv` leads to the weird
behavior that the output of `collect_split_files` (i.e., the existing
split_files) is replaced with the output of the checkpoint (i.e., the
directory `output_path/subfolder`). No workaround, need to delete the
intermediate files (or find out how this is an user error).