# ZSH Shell Setup (Optional)

Setting up ZSH can enhance your command-line experience on any node.

## Installation

```bash
sudo apt-get update
sudo apt-get install -y zsh zsh-syntax-highlighting
```

Run the Oh My Zsh installation script:

```bash
sh -c "\$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Configuration

In the new ZSH shell, execute the following:

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions \\
    \${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

echo "source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ~/.zshrc

sed -i 's/^ZSH_THEME="[^"]*"/ZSH_THEME="lukerandall"/' ~/.zshrc

sed -i '/^plugins=(git)/c\\plugins=(\\n  git\\n  zsh-autosuggestions\\n  helm\\n  kubectl\\n)' ~/.zshrc
```

Restart your terminal or source the `.zshrc` file:

```bash
source ~/.zshrc
```
