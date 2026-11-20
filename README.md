# comfyui-wheels

Prebuilt CUDA wheels consumed by the
[comfyui-runpod](https://github.com/DavidSmithZA/comfyui-runpod) images.

This repo holds **no source and no code**. It exists only so those images can
`ADD --checksum` a wheel from a release asset: comfyui-runpod is private, and a
private repo's release assets need an `Authorization` header that a docker build
does not have. Every wheel here is a build of public, permissively licensed
source, so nothing is exposed by hosting it publicly.

Wheels are never committed to git — they are **release assets only**, so cloning
this repo stays cheap.

## Releases

Tag grammar: `<package>-<short source commit>-<cuNNN>-torch<version>`.

| Tag | Package | Links against |
|---|---|---|
| [`sage-40d3586-cu130-torch2.13.0`](https://github.com/DavidSmithZA/comfyui-wheels/releases/tag/sage-40d3586-cu130-torch2.13.0) | SageAttention 2.2.0 | torch 2.13.0 + cu130, cp310, linux_x86_64 |

## Provenance

Every release's notes must record enough to rebuild it exactly:

- the asset's sha256
- the full upstream source commit, and the repo it came from
- the torch version and index URL it links, and the python tag
- the `TORCH_CUDA_ARCH_LIST` actually used
- which arches were verified how (cubins present vs. a real kernel launch)
- the build machine's GPU and the date

## Do not delete or retag a published release

The comfyui-runpod Dockerfiles pin these URLs by checksum. A deleted or retagged
release breaks image builds. Supersede with a new tag instead.

## How they are built

By `tools/build_sage_wheel.sh` in comfyui-runpod, following the runbook at
`tools/BUILD_SAGE_WHEEL.md` there. Not by CI: these builds need more cores and
RAM than a GitHub-hosted runner has, and a GPU to prove the result launches.
