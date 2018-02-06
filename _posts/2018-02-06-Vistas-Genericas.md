---
layout: post
title: Vistas Genéricas
---

#### Lectura de ~n minutos

En el trabajo tenemos webs internas para la empresa hechas en .NET full framework y hace relativamente poco nos dimos cuenta que teníamos muchas webs del mismo tipo.

Siempre tablas para mostrar información de distinto tipo y uno o dos botones que hacían alguna pequeña acción sobre el registro vía ajax o meramente para seleccionar para más adelante realizar otra acción sobre todos los elementos seleccionados. Para ello siempre haciamos una vista distinta para cada modelo... Hasta que vimos el patrón que se repite siempre. Un modelo, misma vista con mismo comportamiento. Así que nos pusimos manos a la obra y crear una vista parcial genérica para estos casos:

```
@model List<object>
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
@{
    ViewBag.Title = "View";
}

<h2>View</h2>
<table id="genericTable" class="table table-hover">
    <thead>
        <tr>
            @{
                foreach(string property in ViewBag.Properties)
                {
                    <th>@property</th>
                }
                if (ViewBag.Actions != null)
                {
                    <th>Actions</th>
                }
            }
        </tr>
    </thead>
    <tbody>
        @{
            for(int i = 0; i < Model.Count; i++)
            {
                <tr>
                @{
                    foreach (string property in ViewBag.Properties)
                    {
                        <td>@Model[i].GetType().GetProperty(property).GetValue(Model[i])</td>
                    }
                    if (ViewBag.Actions != null)
                    {
                        <td>@ViewBag.Actions[i]</td>
                    }
                }
                </tr>
            }
        }
    </tbody>
</table>
```

Esta vista requiere de un listado de objetos a mostrar, y en el `ViewBag` ponemos las posibles acciones que se pueden realizar sobre cada elemento y los nombres de las columnas que se desean mostrar.

El valor de las propiedades es accedido a través de reflexión. En este punto se debe tener cuidado al usar este tipo de vistas, la reflexión es costoso en tiempo de ejecución y si además la propiedad del objeto en cuestión necesita proceso adicional puede crear vistas lentas.

A esta vista se le puede llamar desde distintos controladores, como por ejemplo:

```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using ViewGenerator.Models;
using Microsoft.AspNetCore.Mvc.Rendering;

namespace ViewGenerator.Controllers
{
    public class PersonController:Controller
    {
        private IList<object> people;

        // GET: Person
        public ActionResult Index()
        {
            people = new List<object>()
            {
                new Person("Maria",12),
                new Person("Jhon",3)
            };
            List<string> Properties = new List<string>
            {
                "Name",
                "Age"
            };

            IList<TagBuilder> actions = new List<TagBuilder>();
            foreach(Person person in people)
            {
                TagBuilder builder = new TagBuilder("button");
                builder.AddCssClass("btn btn-primary");
                builder.InnerHtml.Append("Say my name");
                builder.Attributes.Add("onclick","alert(\'"+person.Name+"\')");
                actions.Add(builder);
            }
            ViewBag.Properties = Properties;
            ViewBag.Actions = actions;
            return View("View",people);
        }
    }
}
```

En este caso además de mostrar el nombre y la edad añadimos un botón con `TagBuilder`que ejecuta un alert de *javascript* que en otras ocasiones puede ser cualquier llamada a la función que deseemos.

[Aquí](https://github.com/44r0n/GenericView.git) está el proyecto completo de ejemplo hecho como siempre en .net core.

Nos vemos en las próximas líneas.
