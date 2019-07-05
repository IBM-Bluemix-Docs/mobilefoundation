---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation plans, migration of plans

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

# Migrar el plan de servicio de Mobile Foundation

Las instancias de Mobile Foundation creadas utilizando los planes en desuso deben actualizarse en los nuevos planes. Es posible que la actualización del plan también sea necesaria en función del uso de la instancia.
{: shortdesc}

## Caso de ejemplo de muestra: Migrar del plan Professional Per Device al plan Professional 1 Application

1. En el panel de control de IBM Cloud, seleccione la instancia de servicio de IBM Mobile Foundation que desee migrar.
2. Seleccione **Plan** en la navegación.
   ![Plan de Mobile Foundation existente](images/existing-plan.png)
3. En los planes de tarifas de la lista, seleccione Professional 1 Application.
![Plan de Mobile Foundation nuevo](images/new-plan.png)
4. Pulse en el botón **Guardar** y confirme la migración del plan.
     La migración a Professional 1 Application ya se ha completado y todos los datos existentes se conservan. La facturación se ha modificado y ya no hay tiempo de inactividad.
5. Tras la migración del plan, es necesario volver a crear la instancia de Mobile Foundation desde el panel de control de servicio para que la configuración correcta entre en vigor. Esta actualización requiere un tiempo de inactividad breve. Deberá realizar una planificación para el tiempo de inactividad. Seleccione **Gestionar** desde la navegación y pulse **Volver a crear**.

Si dispone de uno de los planes en desuso, deberá migrar a un plan nuevo.
{: note}

## Migraciones de plan soportadas

* El plan *Developer* en desuso solo se puede actualizar al nuevo plan *Developer*.
* El plan *Developer Pro* (en desuso) solo se puede actualizar a los planes *Professional Per Device* o *Professional 1 Application*.
* El plan *Professional Per Capacity* (en desuso) solo se puede actualizar a los planes *Professional Per Device* o *Professional 1 Application*.
* El plan *Professional Per Device* solo se puede actualizar al plan *Professional 1 Application*.
* El plan *Professional 1 Application* solo se puede actualizar al plan *Professional Per Device*.
* No se ofrece soporte a la actualización del plan para el plan *Developer* nuevo.
