Currently kubectl doesn't easily show which GCP project and cluster you're working on. This solution adds relevant info to the Bash / Z-Shell prompt.

Based on: https://pracucci.com/display-the-current-kubelet-context-in-the-bash-prompt.html

Add to your `.bashrc` or `.zshrc`:

````bash
kube_prompt()
{
   kubectl_current_context=$(kubectl config current-context)
   kubectl_project=$(echo $kubectl_current_context | cut -d '_' -f 2)
   kubectl_cluster=$(echo $kubectl_current_context | cut -d '_' -f 4)
   kubectl_prompt="k8s:($kubectl_project|$kubectl_cluster)"
   echo $kubectl_prompt
}
````

## Add `kube_prompt` to the prompt variable

### Z-Shell config example

````bash
PROMPT='[%D{%F %T}] ${ret_status}%{$fg_bold[green]%}%p%{$fg[cyan]%}%2d %{$fg_bold[green]%}$(kube_prompt) %{$fg_bold[blue]%}$(git_prompt_info)$(hg_prompt_info)%{$fg_bold[blue]%} % %{$reset_color%}'
````
![kubectl example](images/kubectl_prompt_zsh.png?raw=true "Kubectl Z-Shell example")

Or just:

````bash
PROMPT="${PROMPT} $(kube_prompt) "
````

### Bash config example

````bash
PS1="($PS1) $(kube_prompt) "
````

