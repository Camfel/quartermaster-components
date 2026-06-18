# Quartermaster Components

Curated component catalog for [Quartermaster](https://github.com/Camfel/quartermaster).
Each component is a self-contained `stack.yaml` that defines one or more services.

## Available components

| Component | Version | Description |
|-----------|---------|-------------|
| `dashboard` | v1.0 | Web GUI for monitoring services and health |
| `media-stack` | v1.0 | Full Arr suite + Jellyfin for media automation |
| `vpn` | v1.0 | VPN gateway via Gluetun — secrets-backed credentials, WireGuard + OpenVPN |
| `ingress` | v1.0 | Reverse proxy via Caddy with automatic Let's Encrypt TLS |

## Usage

```bash
# List available components
qm components list

# Enable a component
qm enable dashboard
qm enable media-stack

# Disable a component
qm disable dashboard
```

## Directory layout

```
<component>/<version>/stack.yaml
```

Quartermaster fetches the stack from this repo and merges it with user-defined
stacks. User services take precedence on name conflicts.

## Configuration model

Each component ships with **built-in defaults** in its `stack.yaml`.  These
work immediately — no configuration needed.

To **override** a default, create a ConfigMap with the same name the component
references.  ConfigMaps can be created two ways:

| Method | For whom | Where it lives |
|--------|----------|----------------|
| `qm configmap set <name> <key=value>` or GUI form | Non-technical users | `/etc/quartermaster/configmaps/` |
| `vpn-config.yaml` in your Git repo | Technical users | Your GitOps repo |

**Priority**: Git repo ConfigMaps override local ConfigMaps, which override
built-in component defaults.  A technical user's `vpn-config.yaml` always
wins over a GUI-set value.

### Env var resolution per variable

```
secret (vpn-user)  >  configmap (vpn-config.provider)  >  value: "protonvpn"
   highest                                                 lowest (built-in default)
```
