# Домашнее задание к занятию 5 «Тестирование roles»

## Подготовка к выполнению

1. Установите molecule и его драйвера: `pip3 install "molecule molecule_docker molecule_podman`.
2. Выполните `docker pull aragast/netology:latest` —  это образ с podman, tox и несколькими пайтонами (3.7 и 3.9) внутри.

## Основная часть

Ваша цель — настроить тестирование ваших ролей. 

Задача — сделать сценарии тестирования для vector. 

Ожидаемый результат — все сценарии успешно проходят тестирование ролей.

### Molecule

> 1. Запустите  `molecule test -s ubuntu_xenial` (или с любым другим сценарием, не имеет значения) внутри корневой директории clickhouse-role, посмотрите на вывод команды. Данная команда может отработать с ошибками или не отработать вовсе, это нормально. Наша цель - посмотреть как другие в реальном мире используют молекулу И из чего может состоять сценарий тестирования.

Команда завалилась, тк не выполнялся molecule init:

```
WARNING  Driver docker does not provide a schema.
CRITICAL Failed to validate /home/seregin/scripts/edu/ansible-homeworks/mnt-homeworks/08-ansible-05-testing/playbooks/roles/clickhouse/molecule/centos_7/molecule.yml

["Additional properties are not allowed ('playbooks' was unexpected)"]
```

> 2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.

Добавляем в molecule/default/molecule.yml:

```

```

Запускаем molecule test:

```
WARNING  Driver docker does not provide a schema.
INFO     default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item=centos8)
ok: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/seregin/scripts/edu/ansible-homeworks/mnt-homeworks/08-ansible-05-testing/playbooks/roles/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None) 
skipping: [localhost] => (item=None) 
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'centos8', 'pre_build_image': True}) 
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}) 
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'centos8', 'pre_build_image': True}) 
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}) 
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/quay.io/centos/centos:stream8) 
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/ubuntu:latest) 
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j579243957477.612498', 'results_file': '/root/.ansible_async/j579243957477.612498', 'changed': True, 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j572024848607.612523', 'results_file': '/root/.ansible_async/j572024848607.612523', 'changed': True, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=6    changed=2    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Replace this task with one that validates your content] ******************
ok: [centos8] => {
    "msg": "This is the effective test"
}
ok: [ubuntu] => {
    "msg": "This is the effective test"
}

PLAY RECAP *********************************************************************
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Replace this task with one that validates your content] ******************
ok: [centos8] => {
    "msg": "This is the effective test"
}
ok: [ubuntu] => {
    "msg": "This is the effective test"
}

PLAY RECAP *********************************************************************
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier
WARNING  Skipping, verify playbook not configured.
INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory

```

Получаем на выходе несколько варнингов касааемо неподключенных плейбуков, след. ошибок нет

> 3. Добавьте несколько разных дистрибутивов (oraclelinux:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.

Уже добавлены

> 4. Добавьте несколько assert в verify.yml-файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска и др.). 

```
- name: Verify
  hosts: all
  gather_facts: false
  vars:
    vector_config_path: /etc/vector/vector.yaml
  tasks:
    - name: Get Vector version
      ansible.builtin.command: "vector --version"
      changed_when: false
      register: vector_version
    - name: Assert Vector instalation
      assert:
        that: "'{{ vector_version.rc }}' == '0'"
```

Запускаем тест, где среди прочего видим:

```
TASK [Get Vector version] ******************************************************
ok: [ubuntu]
ok: [centos8]

TASK [Assert Vector instalation] ***********************************************
ok: [centos8] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}
```

5. Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
5. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example).
2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo — путь до корня репозитория с vector-role на вашей файловой системе.
> 3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.

```
py37-ansible210 create: /opt/vector-role/.tox/py37-ansible210
py37-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
py37-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.2.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.5,certifi==2024.8.30,cffi==1.15.1,chardet==5.2.0,charset-normalizer==3.4.0,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.6.0,cryptography==44.0.0,distro==1.9.0,docker==6.1.3,enrich==1.2.7,idna==3.10,importlib-metadata==6.7.0,Jinja2==3.1.4,jmespath==1.0.1,lxml==5.3.0,markdown-it-py==2.2.0,MarkupSafe==2.1.5,mdurl==0.1.2,molecule==3.5.2,molecule-docker==1.1.0,molecule-podman==1.1.0,packaging==24.0,paramiko==2.12.0,pathspec==0.11.2,pluggy==1.2.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.9.0.post0,python-slugify==8.0.4,PyYAML==5.4.1,requests==2.31.0,rich==13.8.1,ruamel.yaml==0.18.6,ruamel.yaml.clib==0.2.8,selinux==0.2.1,six==1.17.0,subprocess-tee==0.3.5,tenacity==8.2.3,text-unidecode==1.3,typing_extensions==4.7.1,urllib3==2.0.7,wcmatch==8.4.1,websocket-client==1.6.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible210 run-test-pre: PYTHONHASHSEED='414598122'
py37-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py37-ansible210/bin/molecule test -s compatibility --destroy always (exited with code 1)
py37-ansible30 create: /opt/vector-role/.tox/py37-ansible30
py37-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
py37-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==1.0.0,ansible-lint==5.1.3,arrow==1.2.3,bcrypt==4.2.1,binaryornot==0.4.4,bracex==2.3.post1,cached-property==1.5.2,Cerberus==1.3.5,certifi==2024.8.30,cffi==1.15.1,chardet==5.2.0,charset-normalizer==3.4.0,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.6.0,cryptography==44.0.0,distro==1.9.0,docker==6.1.3,enrich==1.2.7,idna==3.10,importlib-metadata==6.7.0,Jinja2==3.1.4,jmespath==1.0.1,lxml==5.3.0,markdown-it-py==2.2.0,MarkupSafe==2.1.5,mdurl==0.1.2,molecule==3.5.2,molecule-docker==1.1.0,molecule-podman==1.1.0,packaging==24.0,paramiko==2.12.0,pathspec==0.11.2,pluggy==1.2.0,pycparser==2.21,Pygments==2.17.2,PyNaCl==1.5.0,python-dateutil==2.9.0.post0,python-slugify==8.0.4,PyYAML==5.4.1,requests==2.31.0,rich==13.8.1,ruamel.yaml==0.18.6,ruamel.yaml.clib==0.2.8,selinux==0.2.1,six==1.17.0,subprocess-tee==0.3.5,tenacity==8.2.3,text-unidecode==1.3,typing_extensions==4.7.1,urllib3==2.0.7,wcmatch==8.4.1,websocket-client==1.6.1,yamllint==1.26.3,zipp==3.15.0
py37-ansible30 run-test-pre: PYTHONHASHSEED='414598122'
py37-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py37-ansible30/bin/molecule test -s compatibility --destroy always (exited with code 1)
py39-ansible210 create: /opt/vector-role/.tox/py39-ansible210
py39-ansible210 installdeps: -rtox-requirements.txt, ansible<3.0
py39-ansible210 installed: ansible==2.10.7,ansible-base==2.10.17,ansible-compat==24.10.0,ansible-core==2.15.13,ansible-lint==5.1.3,arrow==1.3.0,attrs==24.2.0,bcrypt==4.2.1,binaryornot==0.4.4,bracex==2.5.post1,Cerberus==1.3.5,certifi==2024.8.30,cffi==1.17.1,chardet==5.2.0,charset-normalizer==3.4.0,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.6.0,cryptography==44.0.0,distro==1.9.0,docker==7.1.0,enrich==1.2.7,idna==3.10,importlib-resources==5.0.7,Jinja2==3.1.4,jmespath==1.0.1,jsonschema==4.23.0,jsonschema-specifications==2024.10.1,lxml==5.3.0,markdown-it-py==3.0.0,MarkupSafe==3.0.2,mdurl==0.1.2,molecule==3.5.2,molecule-docker==1.1.0,molecule-podman==1.1.0,packaging==24.2,paramiko==2.12.0,pathspec==0.12.1,pluggy==1.5.0,pycparser==2.22,Pygments==2.18.0,PyNaCl==1.5.0,python-dateutil==2.9.0.post0,python-slugify==8.0.4,PyYAML==5.4.1,referencing==0.35.1,requests==2.32.3,resolvelib==1.0.1,rich==13.9.4,rpds-py==0.22.3,ruamel.yaml==0.18.6,ruamel.yaml.clib==0.2.12,selinux==0.3.0,six==1.17.0,subprocess-tee==0.4.2,tenacity==9.0.0,text-unidecode==1.3,types-python-dateutil==2.9.0.20241206,typing_extensions==4.12.2,urllib3==2.2.3,wcmatch==10.0,yamllint==1.26.3
py39-ansible210 run-test-pre: PYTHONHASHSEED='414598122'
py39-ansible210 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible210/bin/molecule test -s compatibility --destroy always (exited with code 1)
py39-ansible30 create: /opt/vector-role/.tox/py39-ansible30
py39-ansible30 installdeps: -rtox-requirements.txt, ansible<3.1
py39-ansible30 installed: ansible==3.0.0,ansible-base==2.10.17,ansible-compat==24.10.0,ansible-core==2.15.13,ansible-lint==5.1.3,arrow==1.3.0,attrs==24.2.0,bcrypt==4.2.1,binaryornot==0.4.4,bracex==2.5.post1,Cerberus==1.3.5,certifi==2024.8.30,cffi==1.17.1,chardet==5.2.0,charset-normalizer==3.4.0,click==8.1.7,click-help-colors==0.9.4,cookiecutter==2.6.0,cryptography==44.0.0,distro==1.9.0,docker==7.1.0,enrich==1.2.7,idna==3.10,importlib-resources==5.0.7,Jinja2==3.1.4,jmespath==1.0.1,jsonschema==4.23.0,jsonschema-specifications==2024.10.1,lxml==5.3.0,markdown-it-py==3.0.0,MarkupSafe==3.0.2,mdurl==0.1.2,molecule==3.5.2,molecule-docker==1.1.0,molecule-podman==1.1.0,packaging==24.2,paramiko==2.12.0,pathspec==0.12.1,pluggy==1.5.0,pycparser==2.22,Pygments==2.18.0,PyNaCl==1.5.0,python-dateutil==2.9.0.post0,python-slugify==8.0.4,PyYAML==5.4.1,referencing==0.35.1,requests==2.32.3,resolvelib==1.0.1,rich==13.9.4,rpds-py==0.22.3,ruamel.yaml==0.18.6,ruamel.yaml.clib==0.2.12,selinux==0.3.0,six==1.17.0,subprocess-tee==0.4.2,tenacity==9.0.0,text-unidecode==1.3,types-python-dateutil==2.9.0.20241206,typing_extensions==4.12.2,urllib3==2.2.3,wcmatch==10.0,yamllint==1.26.3
py39-ansible30 run-test-pre: PYTHONHASHSEED='414598122'
py39-ansible30 run-test: commands[0] | molecule test -s compatibility --destroy always
CRITICAL 'molecule/compatibility/molecule.yml' glob failed.  Exiting.
ERROR: InvocationError for command /opt/vector-role/.tox/py39-ansible30/bin/molecule test -s compatibility --destroy always (exited with code 1)
__________________________________________________________________________________________________________________ summary ___________________________________________________________________________________________________________________
ERROR:   py37-ansible210: commands failed
ERROR:   py37-ansible30: commands failed
ERROR:   py39-ansible210: commands failed
ERROR:   py39-ansible30: commands failed
```

Все отработало, кроме уничтожения контейнера, тк его внутри нет

> 5. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.

nano tox-requirements.txt -> убираем строку molecule_docker

`molecule init scenario --driver-name podman light`

> 6. Пропишите правильную команду в `tox.ini`, чтобы запускался облегчённый сценарий.

`    {posargs:molecule test -s light`

8. Запустите команду `tox`. Убедитесь, что всё отработало успешно.
9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в репозитории. Не забудьте указать в ответе теги решений Tox и Molecule заданий. В качестве решения пришлите ссылку на  ваш репозиторий и скриншоты этапов выполнения задания. 

## Необязательная часть

1. Проделайте схожие манипуляции для создания роли LightHouse.
2. Создайте сценарий внутри любой из своих ролей, который умеет поднимать весь стек при помощи всех ролей.
3. Убедитесь в работоспособности своего стека. Создайте отдельный verify.yml, который будет проверять работоспособность интеграции всех инструментов между ними.
4. Выложите свои roles в репозитории.

В качестве решения пришлите ссылки и скриншоты этапов выполнения задания.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.
