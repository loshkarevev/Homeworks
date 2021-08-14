#### 1. Приготовьте свой собственный inventory файл prod.yml
```
---
      elasticsearch:
        hosts:
          elastic1:
            ansible_connection: docker
      kibana:
        hosts:
          kibana1:
            ansible_connection: docker
```
#### 2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает kibana
#### 3. При создании tasks рекомендую использовать модули: get_url, template, unarchive, file
#### 4. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, сгенерировать конфигурацию с параметрами
```
- name: Install kibana
    hosts: kibana
    tasks:
      - name: Upload tar.gz kibana from remote URL
        get_url:
          url: "https://artifacts.elastic.co/downloads/kibana/kibana-{{ kibana_version }}-linux-x86_64.tar.gz"
          dest: "/tmp/kibana-{{ elastic_version }}-linux-x86_64.tar.gz"
          mode: 0755
          timeout: 60
          force: true
          validate_certs: false
        register: get_kibana
        until: get_kibana is succeeded
        tags: kibana
      - name: Create directrory for kibana
        file:
          state: directory
          path: "{{ kibana_home }}"
        tags: kibana
      - name: Extract Kibana in the installation directory
        #become: true
        unarchive:
          copy: false
          src: "/tmp/kibana-{{ kibana_version }}-linux-x86_64.tar.gz"
          dest: "{{ kibana_home }}"
          extra_opts: [--strip-components=1]
          creates: "{{ kibana_home }}/bin/kibana"
        tags:
          - skip_ansible_lint
          - kibana
      - name: Set environment Kibana
        #become: true
        template:
          src: templates/kib.sh.j2
          dest: /etc/profile.d/kib.sh
        tags: kibana
```
#### 5. Запустите ansible-lint site.yml и исправьте ошибки, если они есть
```
root@vagrant:/home/mnt-homeworks/08-ansible-02-playbook/playbook# ansible-lint site.yml
Examining site.yml of type playbook
```
#### 6. Попробуйте запустить playbook на этом окружении с флагом --check
```
root@vagrant:/home/mnt-homeworks/08-ansible-02-playbook/playbook# ansible-playbook -i inventory/prod.yml site.yml --check

PLAY [Install Java] ****************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [elastic001]
ok: [kibana001]

TASK [Set facts for Java 8 vars] ***************************************************************************************************************
ok: [elastic001]
ok: [kibana001]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************
changed: [kibana001]
changed: [elastic001]

TASK [Ensure installation dir exists] **********************************************************************************************************
changed: [elastic001]
changed: [kibana001]

TASK [Extract java in the installation directory] **********************************************************************************************
fatal: [elastic001]: FAILED! => {"changed": false, "msg": "dest '/opt/jdk/11.0.11' must be an existing dir"}
fatal: [kibana001]: FAILED! => {"changed": false, "msg": "dest '/opt/jdk/11.0.11' must be an existing dir"}

PLAY RECAP *************************************************************************************************************************************
elastic001                 : ok=4    changed=2    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
kibana001                  : ok=4    changed=2    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```
#### 7. Запустите playbook на prod.yml окружении с флагом --diff. Убедитесь, что изменения на системе произведены
```
root@vagrant:/home/mnt-homeworks/08-ansible-02-playbook/playbook# ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Java] ******************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
fatal: [elastic001]: UNREACHABLE! => {"changed": false, "msg": "Authentication or permission failure. In some cases, you may have been able to authenticate and did not have permissions on the target directory. Consider changing the remote tmp path in ansible.cfg to a path rooted in \"/tmp\". Failed command was: ( umask 77 && mkdir -p \"` echo ~/.ansible/tmp/ansible-tmp-1628937145.111522-168084436504724 `\" && echo ansible-tmp-1628937145.111522-168084436504724=\"` echo ~/.ansible/tmp/ansible-tmp-1628937145.111522-168084436504724 `\" ), exited with result 1", "unreachable": true}
fatal: [kibana001]: UNREACHABLE! => {"changed": false, "msg": "Authentication or permission failure. In some cases, you may have been able to authenticate and did not have permissions on the target directory. Consider changing the remote tmp path in ansible.cfg to a path rooted in \"/tmp\". Failed command was: ( umask 77 && mkdir -p \"` echo ~/.ansible/tmp/ansible-tmp-1628937145.121384-262217154951225 `\" && echo ansible-tmp-1628937145.121384-262217154951225=\"` echo ~/.ansible/tmp/ansible-tmp-1628937145.121384-262217154951225 `\" ), exited with result 1", "unreachable": true}

PLAY RECAP ***************************************************************************************************************************************************
elastic001                 : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0
kibana001                  : ok=0    changed=0    unreachable=1    failed=0    skipped=0    rescued=0    ignored=0
```
#### 8. Повторно запустите playbook с флагом --diff и убедитесь, что playbook идемпотентен
```
root@vagrant:/home/mnt-homeworks/08-ansible-02-playbook/playbook# ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Java] ****************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [elastic001]
ok: [kibana001]

TASK [Set facts for Java 8 vars] ***************************************************************************************************************
ok: [kibana001]
ok: [elastic001]

TASK [Upload .tar.gz file containing binaries from local storage] ******************************************************************************
ok: [elastic001]
ok: [kibana001]

TASK [Ensure installation dir exists] **********************************************************************************************************
ok: [elastic001]
ok: [kibana001]

TASK [Extract java in the installation directory] **********************************************************************************************
skipping: [elastic001]
skipping: [kibana001]

TASK [Export environment variables] ************************************************************************************************************
ok: [elastic001]
ok: [kibana001]

PLAY [Install Elasticsearch] *******************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [elastic001]

TASK [Upload tar.gz Elasticsearch from remote URL] *********************************************************************************************
ok: [elastic001]

TASK [Create directrory for Elasticsearch] *****************************************************************************************************
ok: [elastic001]

TASK [Extract Elasticsearch in the installation directory] *************************************************************************************
skipping: [elastic001]

TASK [Set environment Elastic] *****************************************************************************************************************
ok: [elastic001]

PLAY [Install Kibana] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [kibana001]

TASK [Upload tar.gz Kibana from remote URL] ****************************************************************************************************
ok: [kibana001]

TASK [Create directrory for Kibana (/opt/kibana/7.12.0)] ***************************************************************************************
ok: [kibana001]

TASK [Extract Kibana in the installation directory] ********************************************************************************************
skipping: [kibana001]

TASK [Set environment Kibana] ******************************************************************************************************************
ok: [kibana001]

PLAY RECAP *************************************************************************************************************************************
elastic001                 : ok=9    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
kibana001                  : ok=9    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```
#### 9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги
#### 10. Готовый playbook выложите в свой репозиторий, в ответ предоставьте ссылку на него
