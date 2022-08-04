<info>

Эти сведения справедливы для кластеров Kubernetes версии 1.23 и выше.

Кластеры более старых версий можно [обновить](../k8s-clusters/update-k8s) для получения таких же возможностей.

</info>

Кластеры Kubernetes в VK Cloud поддерживают технологию единого входа (Single Sign-On, SSO): роли Kubernetes тесно интегрированы с [ролями личного кабинета VK Cloud](../../../additionals/account/concepts/rolesandpermissions#matrica-roley-dlya-servisa-konteynerov).
Таким образом, права, доступные пользователю в кластере Kubernetes, напрямую зависят от роли, назначенной пользователю в личном кабинете VK Cloud.

Это позволяет управлять доступом к кластерам на уровне отдельных проектов в личном кабинете, без необходимости отдельного конфигурирования прав пользователей для личного кабинета и для кластеров Kubernetes. Например, отключение учетной записи пользователя или отзыв роли в личном кабинете, приводит к отзыву прав на доступ в кластер Kubernetes.

## Что нужно знать про Single Sign-On

- Функциональность SSO является неотключаемой.
- Узнать о деталях обновления кластера без SSO до версии с SSO можно [здесь](../k8s-clusters/update-k8s).
- Пользователи с ролью «Администратор проекта» или «Суперадминистратор» могут назначать роли другим пользователям.
- Возможно выдать пользователю административный доступ только для кластеров Kubernetes в проекте.

   Для этого нужно назначить этому пользователю роль «Администратор Kubernetes» вместо других ролей с более широкими полномочиями.

- Пользователи с любой ролью могут [получить kubeconfig и подключиться к кластеру](../k8s-start/connect-k8s), но их права в кластере будут ограничены в соответствии с ролью.

   Чтобы просмотреть права на ресурсы кластера Kubernetes, доступные для определенной роли, подключитесь к кластеру и выполните команду:

   ```bash
   kubectl describe clusterrole <роль в Kubernetes>
   ```

## Описание взаимосвязи ролей

Роли личного кабинета, назначенные пользователям, влияют:

- На [доступность операций с кластерами в личном кабинете](../../../additionals/account/concepts/rolesandpermissions#matrica-roley-dlya-servisa-konteynerov).
- На права доступа пользователей ко всем кластерам в рамках проекта.

Роли личного кабинета и предоставляемые ими права в кластерах Kubernetes перечислены ниже:

<tabs>
<tablist>
<tab>Владелец проекта<br>Администратор проекта<br>Суперадминистратор<br>Администратор Kubernetes</tab>
<tab>Оператор Kubernetes</tab>
<tab>Аудитор Kubernetes</tab>
</tablist>
<tabpanel>

**Соответствующая роль Kubernetes:** `admin`.

Роль `admin` предоставляет административный уровень доступа. Предполагается, что роль будет назначаться в пределах пространства имен (namespace) с помощью связывания ролей (RoleBinding). Дает доступ на чтение и запись к большинству объектов в пространстве имен (включая возможность создавать другие роли и связывания ролей), будучи назначенной таким образом.

Не предоставляет доступ на запись к ресурсной квоте (resource quota) или к самому пространству имен, а также [доступ на запись к эндпойнтам](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#write-access-for-endpoints) (endpoints) кластеров Kubernetes v1.22+.

</tabpanel>
<tabpanel>

**Соответствующая роль Kubernetes:** `edit`.

Роль `edit` предоставляет доступ на чтение и запись к большинству объектов в пространстве имен. Позволяет получить доступ к секретам и запускать поды от имени любого сервисного аккаунта в пространстве имен. Поэтому роль может быть использована, чтобы получить доступ к API от имени любого сервисного аккаунта в пространстве имен.

Не позволяет просматривать или изменять роли и связывания ролей. Не предоставляет [доступ на запись к эндпойнтам](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#write-access-for-endpoints) (endpoints) кластеров Kubernetes v1.22+.

</tabpanel>
<tabpanel>

**Соответствующая роль Kubernetes:** `view`.

Роль `view` предоставляет доступ на чтение к большинству объектов в пространстве имен.

Не позволяет просматривать или изменять роли и связывания ролей. Не позволяет получать доступ к секретам, поскольку в противном случае может получить доступ к учетным данным любого сервисного аккаунта в пространстве имен. Это, в свою очередь, позволило бы получить доступ к API от имени любого сервисного аккаунта в пространстве имен, что в данном случае будет расцениваться как превышение привилегий (privilege escalation).

</tabpanel>
</tabs>

Подробнее о ролях Kubernetes читайте [здесь](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles).