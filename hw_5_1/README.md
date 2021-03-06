# Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения."

---

## Задача 1

**Опишите кратко, как вы поняли: в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.**  
полная виртуализация: гипервизор работает на "голом железе"; гостевая ОС не знает, что она находится в виртуальом окружении  
паравиртуализация: гипервизору требуется ОС для доступа к аппараьным ресурсам, гостевая ОС знает, что она находится в виртуальом окружении - используется модифицированное ядро  
виртуализации на основе ОС: виртуальная машина машина является частью хостовой ОС, но со своим независимыи изолированнм окружением    


## Задача 2

**Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.**  

**Организация серверов:** 
- **физические сервера,**
- **паравиртуализация,**
- **виртуализация уровня ОС.**

**Условия использования:**
- **Высоконагруженная база данных, чувствительная к отказу.**  
  `физический сервер`, т.к. в случае использования паравиртуализации появляется лишняя возможность отказа - ОС, на которой установлен гипервизор 
- **Различные web-приложения.**  
  `виртуализация уровня ОС` - возможность гибкого использования системных ресурсов  
- **Windows системы для использования бухгалтерским отделом.**  
  `паравиртуализация` , для эффективного использования аппаратных ресурсов (подразумевается использование Hyper-V) 
- **Системы, выполняющие высокопроизводительные расчеты на GPU.**  
  `физический сервер`, чтобы не тратить ресурсы на виртуализацию GPU (виртуализация GPU больше подходит для виртуальных рабочих столов)

**Опишите, почему вы выбрали к каждому целевому использованию такую организацию.**




## Задача 3

**Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.**

**Сценарии:**

1. **100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.**  
   'Microsoft Hyper-V', поскольку преобладает Windows based инфраструктура
2. **Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.**  
   'KVM или XEN', т.к. требуется open source
3. **Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.**  
   'vSphere Hypervisor' - бесплатный, совместимый и производительный
4. **Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.**  
   KVM - и гипервизор и тестовые машина на Linux - проще поддерживать


## Задача 4

**Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.**   
Недостатком является сложность в сопровождении гетерогенной среды виртуализации: требуются специалисты широкого профиля или несколько комманд, всегда проще управлять однородными системами, т.к. используются однотипные конфигурации и настройки.  
Для минимизации проблем с гетерогенной средой виртуализации надо иметь "гетерогенных" специалистов и использовать универсальные механизмы управления, подходящие для разнородных систем  
**_Создавали бы вы гетерогенную среду или нет?_** : It depends...  
Гетерогенная среда позволяет гибче использовать ресурсы, в том числе и финансовые. Например, для Windows-окружения использовать Hyper-V, а для Linux-серверов - KVM.  