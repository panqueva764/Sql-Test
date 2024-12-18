# Solución al Problema de Identificación de Nodos en un Árbol SQL

Este repositorio contiene la solución al problema de identificación de los tipos de nodos (Raíz, Interior y Hoja) en un árbol jerárquico representado en una tabla SQL, utilizando MySQL.

## Problema

Dada una tabla `Tree` que contiene dos columnas:

- `id`: identificador único del nodo.
- `p_id`: identificador del nodo padre.

El objetivo es clasificar los nodos en tres tipos:

- **Raíz (Root)**: Nodo sin padre, es decir, `p_id` es `NULL`.
- **Interior (Inner)**: Nodo que tiene al menos un hijo.
- **Hoja (Leaf)**: Nodo que tiene un `p_id` pero no es padre de ningún otro nodo.

### Ejemplo de la Tabla

La tabla de ejemplo `Tree` tiene los siguientes datos:

| id  | p_id |
| --- | ---- |
| 1   | NULL |
| 2   | 1    |
| 3   | 1    |
| 4   | 2    |
| 5   | 2    |

### Resultado Esperado

Al ejecutar la solución, se espera que la consulta devuelva la siguiente tabla con los nodos clasificados:

| id  | Type  |
| --- | ----- |
| 1   | Root  |
| 2   | Inner |
| 3   | Leaf  |
| 4   | Leaf  |
| 5   | Leaf  |

**Explicación del resultado:**

- **Nodo 1**: Es el nodo raíz porque su `p_id` es `NULL` y tiene los nodos hijos 2 y 3.
- **Nodo 2**: Es un nodo interior porque tiene el nodo padre 1 y nodos hijos 4 y 5.
- **Nodos 3, 4 y 5**: Son nodos hoja porque tienen un `p_id`, pero no tienen hijos.

---

## Solución

A continuación, se presenta la solución que incluye la instalación de MySQL, la creación de la base de datos, la creación de la tabla y la consulta para obtener el resultado esperado.

### 1. **Instalar MySQL en Ubuntu**

Si no tienes MySQL instalado en tu sistema, sigue estos pasos para instalarlo en Ubuntu 24. Ejecuta los siguientes comandos en la terminal:

```bash
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql
sudo systemctl status mysql
sudo systemctl enable mysql
sudo mysql_secure_installation // todo no

Acceder a MySQL
Accede a MySQL con el siguiente comando:
sudo mysql

```

```sql

CREATE DATABASE IF NOT EXISTS tree_db;
USE tree_db;

CREATE TABLE IF NOT EXISTS Tree (
    id INT,
    p_id INT
);

INSERT INTO Tree (id, p_id) VALUES
(1, NULL),  -- Nodo raíz
(2, 1),     -- Nodo hijo de 1
(3, 1),     -- Nodo hijo de 1
(4, 2),     -- Nodo hijo de 2
(5, 2);     -- Nodo hijo de 2

SELECT 
    id,
    CASE
        WHEN p_id IS NULL THEN 'Root'                             -- Nodo raíz
        WHEN id IN (SELECT p_id FROM Tree) THEN 'Inner'           -- Nodo interior
        ELSE 'Leaf'                                              -- Nodo hoja
    END AS Type
FROM Tree
ORDER BY FIELD(
    CASE
        WHEN p_id IS NULL THEN 'Root'
        WHEN id IN (SELECT p_id FROM Tree) THEN 'Inner'
        ELSE 'Leaf'
    END,
    'Root', 'Inner', 'Leaf'
);


```
Resultados iguales a los sperados

El resultado al ejecutar la consulta es el siguiente:

| id  | Type  |
| --- | ----- |
| 1   | Root  |
| 2   | Inner |
| 3   | Leaf  |
| 4   | Leaf  |
| 5   | Leaf  |
