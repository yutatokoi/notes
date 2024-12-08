# コンテナ

## コンテナ以前：仮想マシン

Virtualisation allows for multiple operating systems to utilise the same host computer's hardware resources. The hypervisor (AKA Virtual Machine Monitor (VMM)) creates a virtual platform on the host to enable virtualisation (<https://docs.oracle.com/cd/E26996_01/E18549/html/VMUSG1011.html>).

## コンテナの強み

- 軽量で移植性の高いアプリケーション実行環境。アプリケーションとその依存関係を一つのパッケージにまとめることができる。(<https://www.docker.com/ja-jp/resources/what-container/>)
    - 仮想マシンより軽量。全てのコンテナが共有のOSシステムカーネルを使用するため。(<https://www.docker.com/ja-jp/resources/what-container/>)
    - 仮想マシンより早い起動。(<https://www.ibm.com/topics/containerization>)
    - Write once, run everywhere. ホストマシンのOSは関係なく実行できる。(<https://www.docker.com/ja-jp/resources/what-container/>)

## Container security

Compromised container images, container vulnerabilities/misconfiguration, container escape (<https://www.paloaltonetworks.com/cyberpedia/what-is-container-security>)

Solutions:

- Container registry. One source to create and deploy containers from, in order to ensure they can be trusted images.
- Regular scanning containers for vulnerabilities. Outdated libraries with known vulnerabilities can be mitigated by keeping them up to date with patches.
- Host OS security, network security. Not specific to containers but still important regardless.

## Other Resources

- <https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8861307>
- <https://docs.docker.com/get-started/docker-overview/>
- <https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html>
