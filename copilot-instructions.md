# Copilot Instructions

## Language
- Please **respond in Japanese** for all chat conversations.  
- However, code comments should be **written in English**.  

---

## Coding Guidelines

- Actively refactor logic into functions and avoid writing large blocks of inline code.  
- Use **type hints (typing)** for all functions. Please use `Dict[key_type, value_type]`, `List[item_type]`, and `Any` instead of untyped `dict`, `list`, and `any`.  
- Use **Pathlib’s `Path`** instead of `os` for path operations.
- **f-strings** should be used for string formatting.
- Keep the structure **simple and minimal** — strict error handling (e.g., file existence check) is **not required**.
- Comments are **not required frequently**; only add them when clarity is necessary.
- When calling functions, do **not** place a trailing comma after the final argument.  
- When performing any **random or stochastic operation**, always **set a random seed** to ensure reproducibility.
- When using **matplotlib**, always use `plt.subplots()`. Specify both **`ncols=`** and **`nrows=`** explicitly when calling `subplots()`.  
- When using multiple axes in matplotlib, add **`ylabel` only to the leftmost column** and **`xlabel` only to the bottom row**.  
- Use `plt.savefig(..., bbox_inches="tight")` instead of `plt.show()`.
- When calling **user-defined (custom) functions**, always use **explicit keyword arguments**
  (e.g., `func(arg1=value1, arg2=value2)`), and avoid positional-only calls.
  This rule does **not** apply to calls of external libraries or built-in functions. 
- Use **tqdm** for progress bars when iterating over loops or processing batches.
- Manage hyperparameters for the `main` function via **Hydra**, and receive the config as `hydra_cfg: DictConfig`. Validate this Hydra config using **Pydantic**, following the pattern below:

    ```python
    from typing import Any
    from omegaconf import DictConfig, OmegaConf
    from pydantic import BaseModel
    import hydra

    class Config(BaseModel):
        seed: int

    def hydra_to_pydantic(hydra_cfg: DictConfig) -> Config:
        """Converts Hydra config to Pydantic config."""
        config_dict: dict[str, Any] = OmegaConf.to_object(hydra_cfg)
        return Config(**config_dict)

    @hydra.main(version_base=None, config_path="config", config_name="config")
    def main(hydra_cfg: DictConfig) -> None:
        cfg = hydra_to_pydantic(hydra_cfg)
```
