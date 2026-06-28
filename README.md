# brave-origin-flake
A Nix flake for Brave Origin

## Usage
### Without Installing
```bash
nix run github:tekq/brave-origin-flake
```
### Flake
```
{
  description = "Example Nix Flake";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    brave-origin.url = "github:tekq/brave-origin-flake";
  };

  outputs = {
    self,
    nixpkgs,
    ...
  } @ inputs: {
    nixosConfigurations."nixos" = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [
        ./configuration.nix
        
        ({ pkgs, ... }: {
          environment.systemPackages = [
            inputs.brave-origin.packages.${pkgs.stdenv.hostPlatform.system}.default
          ];
        })
      ];
    };
  };
}
```
### Home Manager
```
{
  inputs,
  pkgs,
  ...
}: {
  home.packages = with pkgs; [
    inputs.brave-origin.packages.${pkgs.stdenv.hostPlatform.system}.default
  ];
}
```
