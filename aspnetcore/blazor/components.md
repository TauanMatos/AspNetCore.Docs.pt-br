---
title: Criar e usar ASP.NET Core componentes do Razor
author: guardrex
description: Saiba como criar e usar componentes do Razor, incluindo como associar dados, manipular eventos e gerenciar ciclos de vida do componente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/components
ms.openlocfilehash: a95c186d30eaf342f10ecbe6f7add242d4679a0f
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993410"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="665b3-103">Criar e usar ASP.NET Core componentes do Razor</span><span class="sxs-lookup"><span data-stu-id="665b3-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="665b3-104">De [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="665b3-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="665b3-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="665b3-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="665b3-106">Os aplicativos mais elaborados são criados usando *componentes*.</span><span class="sxs-lookup"><span data-stu-id="665b3-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="665b3-107">Um componente é uma parte independente da interface do usuário (IU), como uma página, uma caixa de diálogo ou um formulário.</span><span class="sxs-lookup"><span data-stu-id="665b3-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="665b3-108">Um componente inclui marcação HTML e a lógica de processamento necessária para injetar dados ou responder a eventos da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="665b3-109">Os componentes são flexíveis e leves.</span><span class="sxs-lookup"><span data-stu-id="665b3-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="665b3-110">Eles podem ser aninhados, reutilizados e compartilhados entre projetos.</span><span class="sxs-lookup"><span data-stu-id="665b3-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="665b3-111">Classes de componente</span><span class="sxs-lookup"><span data-stu-id="665b3-111">Component classes</span></span>

<span data-ttu-id="665b3-112">Os componentes são implementados em arquivos de componente do [Razor](xref:mvc/views/razor) ( *. Razor*) C# usando uma combinação de marcação HTML e.</span><span class="sxs-lookup"><span data-stu-id="665b3-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="665b3-113">Um componente no mais alto é conhecido formalmente como um *componente do Razor*.</span><span class="sxs-lookup"><span data-stu-id="665b3-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="665b3-114">O nome de um componente deve começar com um caractere maiúsculo.</span><span class="sxs-lookup"><span data-stu-id="665b3-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="665b3-115">Por exemplo, *MyCoolComponent. Razor* é válido e *MyCoolComponent. Razor* é inválido.</span><span class="sxs-lookup"><span data-stu-id="665b3-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="665b3-116">Os componentes podem ser criados usando a extensão de arquivo *. cshtml* , desde que os arquivos sejam identificados como arquivos de componente do `_RazorComponentInclude` Razor usando a Propriedade MSBuild.</span><span class="sxs-lookup"><span data-stu-id="665b3-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="665b3-117">Por exemplo, um aplicativo que especifica que todos os arquivos *. cshtml* na pasta *páginas* devem ser tratados como arquivos de componentes do Razor:</span><span class="sxs-lookup"><span data-stu-id="665b3-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="665b3-118">A interface do usuário para um componente é definida usando HTML.</span><span class="sxs-lookup"><span data-stu-id="665b3-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="665b3-119">A lógica de renderização dinâmica (por exemplo, loops, condicionais, expressões) é adicionada usando uma sintaxe de C# inserida chamada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="665b3-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="665b3-120">Quando um aplicativo é compilado, a marcação HTML e C# a lógica de renderização são convertidas em uma classe de componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="665b3-121">O nome da classe gerada corresponde ao nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="665b3-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="665b3-122">Os membros da classe de componente são definidos em um bloco `@code`.</span><span class="sxs-lookup"><span data-stu-id="665b3-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="665b3-123">`@code` No bloco, estado do componente (Propriedades, campos) é especificado com métodos para manipulação de eventos ou para definir outra lógica de componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="665b3-124">Mais de um bloco de `@code` é permitido.</span><span class="sxs-lookup"><span data-stu-id="665b3-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="665b3-125">Nas visualizações anteriores do ASP.NET Core 3,0, `@functions` os blocos foram usados para a mesma finalidade que `@code` os blocos nos componentes do Razor.</span><span class="sxs-lookup"><span data-stu-id="665b3-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="665b3-126">`@functions`os blocos continuam a funcionar em componentes do Razor, mas é recomendável usar o `@code` bloco no ASP.NET Core 3,0 Preview 6 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="665b3-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="665b3-127">Os membros do componente podem ser usados como parte da lógica de renderização do C# componente usando expressões que `@`começam com.</span><span class="sxs-lookup"><span data-stu-id="665b3-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="665b3-128">Por exemplo, um C# campo é renderizado pela prefixação `@` para o nome do campo.</span><span class="sxs-lookup"><span data-stu-id="665b3-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="665b3-129">O exemplo a seguir avalia e renderiza:</span><span class="sxs-lookup"><span data-stu-id="665b3-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="665b3-130">`_headingFontStyle`para o valor da propriedade de `font-style`CSS para.</span><span class="sxs-lookup"><span data-stu-id="665b3-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="665b3-131">`_headingText`para o conteúdo do `<h1>` elemento.</span><span class="sxs-lookup"><span data-stu-id="665b3-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="665b3-132">Depois que o componente é processado inicialmente, o componente regenera sua árvore de renderização em resposta a eventos.</span><span class="sxs-lookup"><span data-stu-id="665b3-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="665b3-133">Em seguida, ele compara a nova árvore de renderização com a anterior e aplica quaisquer modificações ao Modelo de Objeto do Documento do navegador (DOM).</span><span class="sxs-lookup"><span data-stu-id="665b3-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="665b3-134">Os componentes são C# classes comuns e podem ser colocados em qualquer lugar dentro de um projeto.</span><span class="sxs-lookup"><span data-stu-id="665b3-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="665b3-135">Os componentes que produzem páginas da Web geralmente residem na pasta *páginas* .</span><span class="sxs-lookup"><span data-stu-id="665b3-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="665b3-136">Os componentes que não são de página são frequentemente colocados na pasta *compartilhada* ou em uma pasta personalizada adicionada ao projeto.</span><span class="sxs-lookup"><span data-stu-id="665b3-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="665b3-137">Para usar uma pasta personalizada, adicione o namespace da pasta personalizada ao componente pai ou ao arquivo *_Imports. Razor* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="665b3-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="665b3-138">Por exemplo, o namespace a seguir torna os componentes em uma pasta *componentes* disponíveis quando o namespace raiz do `WebApplication`aplicativo é:</span><span class="sxs-lookup"><span data-stu-id="665b3-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="665b3-139">Integrar componentes em aplicativos Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="665b3-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="665b3-140">Use componentes com os aplicativos Razor Pages e MVC existentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="665b3-141">Não é necessário reescrever páginas ou exibições existentes para usar os componentes do Razor.</span><span class="sxs-lookup"><span data-stu-id="665b3-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="665b3-142">Quando a página ou a exibição é renderizada, os componentes são renderizados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="665b3-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="665b3-143">Para renderizar um componente de uma página ou exibição, use `RenderComponentAsync<TComponent>` o método auxiliar HTML:</span><span class="sxs-lookup"><span data-stu-id="665b3-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="665b3-144">Embora as páginas e exibições possam usar componentes, o inverso não é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="665b3-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="665b3-145">Os componentes não podem usar cenários específicos de exibição e de página, como exibições parciais e seções.</span><span class="sxs-lookup"><span data-stu-id="665b3-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="665b3-146">Para usar a lógica da exibição parcial em um componente, desfatore a lógica de exibição parcial em um componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="665b3-147">Para obter mais informações sobre como os componentes são renderizados e o estado do componente é gerenciado no mais alto aplicativo do <xref:blazor/hosting-models> lado do servidor, consulte o artigo.</span><span class="sxs-lookup"><span data-stu-id="665b3-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="665b3-148">Usar componentes</span><span class="sxs-lookup"><span data-stu-id="665b3-148">Use components</span></span>

<span data-ttu-id="665b3-149">Os componentes podem incluir outros componentes, declarando-os usando a sintaxe do elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="665b3-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="665b3-150">A marcação para uso de um componente é semelhante a uma marca HTML, em que o nome da marca é o tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="665b3-151">A associação de atributo diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="665b3-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="665b3-152">Por exemplo, `@bind` é válido e `@Bind` é inválido.</span><span class="sxs-lookup"><span data-stu-id="665b3-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="665b3-153">A marcação a seguir no *index. Razor* renderiza uma `HeadingComponent` instância:</span><span class="sxs-lookup"><span data-stu-id="665b3-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="665b3-154">*Componentes/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="665b3-155">Se um componente contiver um elemento HTML com uma letra maiúscula ou minúscula que não corresponda a um nome de componente, um aviso será emitido indicando que o elemento tem um nome inesperado.</span><span class="sxs-lookup"><span data-stu-id="665b3-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="665b3-156">A adição `@using` de uma instrução para o namespace do componente torna o componente disponível, o que remove o aviso.</span><span class="sxs-lookup"><span data-stu-id="665b3-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="665b3-157">Parâmetros do componente</span><span class="sxs-lookup"><span data-stu-id="665b3-157">Component parameters</span></span>

<span data-ttu-id="665b3-158">Os componentes podem ter *parâmetros de componente*, que são definidos usando propriedades públicas na classe de componente `[Parameter]` com o atributo.</span><span class="sxs-lookup"><span data-stu-id="665b3-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="665b3-159">Use atributos para especificar argumentos para um componente na marcação.</span><span class="sxs-lookup"><span data-stu-id="665b3-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="665b3-160">*Componentes/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="665b3-161">No exemplo a seguir, `ParentComponent` define o valor `Title` da Propriedade do `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="665b3-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="665b3-162">*Páginas/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="665b3-163">Conteúdo filho</span><span class="sxs-lookup"><span data-stu-id="665b3-163">Child content</span></span>

<span data-ttu-id="665b3-164">Os componentes podem definir o conteúdo de outro componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-164">Components can set the content of another component.</span></span> <span data-ttu-id="665b3-165">O componente de atribuição fornece o conteúdo entre as marcas que especificam o componente de recebimento.</span><span class="sxs-lookup"><span data-stu-id="665b3-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="665b3-166">No exemplo a seguir, o `ChildComponent` tem uma `ChildContent` propriedade que representa um `RenderFragment`, que representa um segmento de interface do usuário a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="665b3-167">O valor de `ChildContent` é posicionado na marcação do componente onde o conteúdo deve ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="665b3-168">O valor de `ChildContent` é recebido do componente pai e renderizado dentro do `panel-body`painel de inicialização.</span><span class="sxs-lookup"><span data-stu-id="665b3-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="665b3-169">*Componentes/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="665b3-170">A propriedade que recebe `RenderFragment` o conteúdo deve ser `ChildContent` nomeada por convenção.</span><span class="sxs-lookup"><span data-stu-id="665b3-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="665b3-171">O seguinte `ParentComponent` pode fornecer conteúdo para renderizar `ChildComponent` o colocando o conteúdo dentro das `<ChildComponent>` marcas.</span><span class="sxs-lookup"><span data-stu-id="665b3-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="665b3-172">*Páginas/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="665b3-173">Atributo nivelamento e parâmetros arbitrários</span><span class="sxs-lookup"><span data-stu-id="665b3-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="665b3-174">Os componentes podem capturar e renderizar atributos adicionais além dos parâmetros declarados do componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="665b3-175">Atributos adicionais podem ser capturados em um dicionário e, em seguida, *splatted* em um elemento quando o componente [@attributes](xref:mvc/views/razor#attributes) é renderizado usando a diretiva Razor.</span><span class="sxs-lookup"><span data-stu-id="665b3-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="665b3-176">Esse cenário é útil ao definir um componente que produz um elemento de marcação que dá suporte a uma variedade de personalizações.</span><span class="sxs-lookup"><span data-stu-id="665b3-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="665b3-177">Por exemplo, pode ser entediante definir atributos separadamente para um `<input>` que dê suporte a muitos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="665b3-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="665b3-178">No exemplo a seguir, o primeiro `<input>` elemento (`id="useIndividualParams"`) usa parâmetros de componente individuais, enquanto o `<input>` segundo elemento`id="useAttributesDict"`() usa o atributo nivelamento:</span><span class="sxs-lookup"><span data-stu-id="665b3-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "true" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="665b3-179">O tipo do parâmetro deve implementar `IEnumerable<KeyValuePair<string, object>>` com chaves de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="665b3-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="665b3-180">Usar `IReadOnlyDictionary<string, object>` também é uma opção nesse cenário.</span><span class="sxs-lookup"><span data-stu-id="665b3-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="665b3-181">Os elementos `<input>` renderizados usando ambas as abordagens são idênticos:</span><span class="sxs-lookup"><span data-stu-id="665b3-181">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="true"
       size="50">
```

<span data-ttu-id="665b3-182">Para aceitar atributos arbitrários, defina um parâmetro de componente `[Parameter]` usando o atributo `CaptureUnmatchedValues` com a propriedade `true`definida como:</span><span class="sxs-lookup"><span data-stu-id="665b3-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedAttributes = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="665b3-183">A `CaptureUnmatchedValues` Propriedade on `[Parameter]` permite que o parâmetro corresponda a todos os atributos que não correspondem a nenhum outro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="665b3-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="665b3-184">Um componente só pode definir um único parâmetro com `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="665b3-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="665b3-185">O tipo de propriedade usado `CaptureUnmatchedValues` com deve ser atribuível de `Dictionary<string, object>` com chaves de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="665b3-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="665b3-186">`IEnumerable<KeyValuePair<string, object>>`ou `IReadOnlyDictionary<string, object>` também são opções neste cenário.</span><span class="sxs-lookup"><span data-stu-id="665b3-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="665b3-187">Associação de dados</span><span class="sxs-lookup"><span data-stu-id="665b3-187">Data binding</span></span>

<span data-ttu-id="665b3-188">A ligação de dados com os componentes e os elementos DOM é [@bind](xref:mvc/views/razor#bind) realizada com o atributo.</span><span class="sxs-lookup"><span data-stu-id="665b3-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="665b3-189">O exemplo a seguir associa o `_italicsCheck` campo ao estado marcado da caixa de seleção:</span><span class="sxs-lookup"><span data-stu-id="665b3-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="665b3-190">Quando a caixa de seleção é marcada e desmarcada, o valor da propriedade `true` é `false`atualizado para e, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="665b3-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="665b3-191">A caixa de seleção é atualizada na interface do usuário somente quando o componente é renderizado, não em resposta à alteração do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="665b3-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="665b3-192">Como os componentes são renderizados após a execução do código do manipulador de eventos, as atualizações de propriedade geralmente são refletidas na interface do usuário imediatamente.</span><span class="sxs-lookup"><span data-stu-id="665b3-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="665b3-193">Usar `@bind` with a `CurrentValue` Property (`<input @bind="CurrentValue" />`) é essencialmente equivalente ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="665b3-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="665b3-194">Quando o componente é renderizado, `value` o do elemento de entrada vem `CurrentValue` da propriedade.</span><span class="sxs-lookup"><span data-stu-id="665b3-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="665b3-195">Quando o usuário digita na caixa de texto, o `onchange` evento é acionado e `CurrentValue` a propriedade é definida como o valor alterado.</span><span class="sxs-lookup"><span data-stu-id="665b3-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="665b3-196">Na realidade, a geração de código é um pouco mais complexa `@bind` porque lida com alguns casos em que as conversões de tipo são executadas.</span><span class="sxs-lookup"><span data-stu-id="665b3-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="665b3-197">Em princípio, `@bind` associa o valor atual de uma expressão a um `value` atributo e manipula as alterações usando o manipulador registrado.</span><span class="sxs-lookup"><span data-stu-id="665b3-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="665b3-198">Além de `onchange` manipular eventos com [@bind-value](xref:mvc/views/razor#bind) `@bind` sintaxe, uma propriedade ou campo pode ser associado usando outros eventos, especificando um atributo com um `event` parâmetro ([@bind-value:event](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="665b3-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="665b3-199">O exemplo a seguir associa a `CurrentValue` propriedade para o `oninput` evento:</span><span class="sxs-lookup"><span data-stu-id="665b3-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="665b3-200">Ao `onchange`contrário de, que é acionado quando o `oninput` elemento perde o foco, é acionado quando o valor da caixa de texto é alterado.</span><span class="sxs-lookup"><span data-stu-id="665b3-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="665b3-201">**Globalização**</span><span class="sxs-lookup"><span data-stu-id="665b3-201">**Globalization**</span></span>

<span data-ttu-id="665b3-202">`@bind`os valores são formatados para exibição e analisados usando as regras da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="665b3-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="665b3-203">A cultura atual pode ser acessada <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> a partir da propriedade.</span><span class="sxs-lookup"><span data-stu-id="665b3-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="665b3-204">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) é usado para os seguintes tipos de campo`<input type="{TYPE}" />`():</span><span class="sxs-lookup"><span data-stu-id="665b3-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="665b3-205">Os tipos de campo anteriores:</span><span class="sxs-lookup"><span data-stu-id="665b3-205">The preceding field types:</span></span>

* <span data-ttu-id="665b3-206">São exibidos usando suas regras de formatação baseadas em navegador apropriadas.</span><span class="sxs-lookup"><span data-stu-id="665b3-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="665b3-207">Não pode conter texto de forma livre.</span><span class="sxs-lookup"><span data-stu-id="665b3-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="665b3-208">Fornecer características de interação do usuário com base na implementação do navegador.</span><span class="sxs-lookup"><span data-stu-id="665b3-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="665b3-209">Os seguintes tipos de campo têm requisitos de formatação específicos e atualmente não são compatíveis com o mais grande, pois não têm suporte de todos os principais navegadores:</span><span class="sxs-lookup"><span data-stu-id="665b3-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="665b3-210">`@bind`dá suporte ao <xref:System.Globalization.CultureInfo?displayProperty=fullName> `@bind:culture` parâmetro para fornecer um para análise e formatação de um valor.</span><span class="sxs-lookup"><span data-stu-id="665b3-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="665b3-211">Não é recomendável especificar uma cultura ao `date` usar `number` os tipos de campo e.</span><span class="sxs-lookup"><span data-stu-id="665b3-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="665b3-212">`date`e `number` têm suporte interno de mais alto que fornece a cultura necessária.</span><span class="sxs-lookup"><span data-stu-id="665b3-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="665b3-213">Para obter informações sobre como definir a cultura do usuário, consulte a seção [localização](#localization) .</span><span class="sxs-lookup"><span data-stu-id="665b3-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="665b3-214">**Formatar cadeias de caracteres**</span><span class="sxs-lookup"><span data-stu-id="665b3-214">**Format strings**</span></span>

<span data-ttu-id="665b3-215">A vinculação de dados <xref:System.DateTime> funciona com cadeias de caracteres de formato usando. [@bind:format](xref:mvc/views/razor#bind)</span><span class="sxs-lookup"><span data-stu-id="665b3-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="665b3-216">Outras expressões de formato, como formatos de moeda ou número, não estão disponíveis no momento.</span><span class="sxs-lookup"><span data-stu-id="665b3-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="665b3-217">No código anterior, o `<input>` tipo de campo do elemento (`type`) usa como `text`padrão.</span><span class="sxs-lookup"><span data-stu-id="665b3-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="665b3-218">`@bind:format`tem suporte para ligar os seguintes tipos .NET:</span><span class="sxs-lookup"><span data-stu-id="665b3-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="665b3-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="665b3-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="665b3-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="665b3-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="665b3-221">O `@bind:format` atributo especifica o formato de data a ser aplicado `value` ao do `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="665b3-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="665b3-222">O formato também é usado para analisar o valor quando ocorre `onchange` um evento.</span><span class="sxs-lookup"><span data-stu-id="665b3-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="665b3-223">Não é recomendável especificar `date` um formato para o tipo de campo porque o mais alto tem suporte interno para formatar datas.</span><span class="sxs-lookup"><span data-stu-id="665b3-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="665b3-224">**Parâmetros do componente**</span><span class="sxs-lookup"><span data-stu-id="665b3-224">**Component parameters**</span></span>

<span data-ttu-id="665b3-225">A associação reconhece os parâmetros do `@bind-{property}` componente, onde pode associar um valor de propriedade entre componentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="665b3-226">O componente filho a seguir`ChildComponent`() tem `Year` um parâmetro de `YearChanged` componente e um retorno de chamada:</span><span class="sxs-lookup"><span data-stu-id="665b3-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="665b3-227">`EventCallback<T>`é explicado na seção [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="665b3-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="665b3-228">O componente pai a seguir `ChildComponent` usa e associa o `ParentYear` parâmetro `Year` do pai ao parâmetro no componente filho:</span><span class="sxs-lookup"><span data-stu-id="665b3-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="665b3-229">O carregamento `ParentComponent` do produz a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="665b3-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="665b3-230">`ParentYear` Se o valor da propriedade for alterado selecionando o botão `ParentComponent`no `ChildComponent` , a `Year` Propriedade do será atualizada.</span><span class="sxs-lookup"><span data-stu-id="665b3-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="665b3-231">O novo valor de `Year` é renderizado na interface do usuário `ParentComponent` quando o é rerenderizado:</span><span class="sxs-lookup"><span data-stu-id="665b3-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="665b3-232">O `Year` parâmetro é ligável porque tem um evento `YearChanged` complementar que `Year` corresponde ao tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="665b3-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="665b3-233">Por convenção, `<ChildComponent @bind-Year="ParentYear" />` é essencialmente equivalente a escrever:</span><span class="sxs-lookup"><span data-stu-id="665b3-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="665b3-234">Em geral, uma propriedade pode ser associada a um manipulador de eventos correspondente `@bind-property:event` usando o atributo.</span><span class="sxs-lookup"><span data-stu-id="665b3-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="665b3-235">Por exemplo, a propriedade `MyProp` pode ser associada ao `MyEventHandler` uso dos dois atributos a seguir:</span><span class="sxs-lookup"><span data-stu-id="665b3-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="665b3-236">Manipulação de eventos</span><span class="sxs-lookup"><span data-stu-id="665b3-236">Event handling</span></span>

<span data-ttu-id="665b3-237">Os componentes do Razor fornecem recursos de manipulação de eventos.</span><span class="sxs-lookup"><span data-stu-id="665b3-237">Razor components provide event handling features.</span></span> <span data-ttu-id="665b3-238">Para um atributo de elemento HTML `on{event}` chamado (por exemplo `onclick` , `onsubmit`e) com um valor de tipo delegado, os componentes do Razor tratam o valor do atributo como um manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="665b3-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="665b3-239">O nome do atributo é sempre formatado [ @on{Event}](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="665b3-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="665b3-240">O código a seguir chama `UpdateHeading` o método quando o botão é selecionado na interface do usuário:</span><span class="sxs-lookup"><span data-stu-id="665b3-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="665b3-241">O código a seguir chama `CheckChanged` o método quando a caixa de seleção é alterada na interface do usuário:</span><span class="sxs-lookup"><span data-stu-id="665b3-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="665b3-242">Os manipuladores de eventos também podem ser assíncronos <xref:System.Threading.Tasks.Task>e retornar um.</span><span class="sxs-lookup"><span data-stu-id="665b3-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="665b3-243">Não há necessidade de chamar `StateHasChanged()`manualmente.</span><span class="sxs-lookup"><span data-stu-id="665b3-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="665b3-244">As exceções são registradas quando ocorrem.</span><span class="sxs-lookup"><span data-stu-id="665b3-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="665b3-245">No exemplo a seguir, `UpdateHeading` é chamado de forma assíncrona quando o botão é selecionado:</span><span class="sxs-lookup"><span data-stu-id="665b3-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="665b3-246">Tipos de argumento de evento</span><span class="sxs-lookup"><span data-stu-id="665b3-246">Event argument types</span></span>

<span data-ttu-id="665b3-247">Para alguns eventos, são permitidos tipos de argumento de evento.</span><span class="sxs-lookup"><span data-stu-id="665b3-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="665b3-248">Se o acesso a um desses tipos de evento não for necessário, ele não será necessário na chamada do método.</span><span class="sxs-lookup"><span data-stu-id="665b3-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="665b3-249">Os [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) com suporte são mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="665b3-249">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="665b3-250">evento</span><span class="sxs-lookup"><span data-stu-id="665b3-250">Event</span></span> | <span data-ttu-id="665b3-251">Classe</span><span class="sxs-lookup"><span data-stu-id="665b3-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="665b3-252">Área de Transferência</span><span class="sxs-lookup"><span data-stu-id="665b3-252">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="665b3-253">Arraste</span><span class="sxs-lookup"><span data-stu-id="665b3-253">Drag</span></span>  | <span data-ttu-id="665b3-254">`UIDragEventArgs`é usado para manter os dados arrastados durante uma operação de arrastar e soltar e pode conter um `UIDataTransferItem`ou mais. &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="665b3-254">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="665b3-255">`UIDataTransferItem`representa um item de dados de arrastar.</span><span class="sxs-lookup"><span data-stu-id="665b3-255">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="665b3-256">Erro</span><span class="sxs-lookup"><span data-stu-id="665b3-256">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="665b3-257">Foco</span><span class="sxs-lookup"><span data-stu-id="665b3-257">Focus</span></span> | <span data-ttu-id="665b3-258">`UIFocusEventArgs`Não inclui suporte para `relatedTarget`. &ndash;</span><span class="sxs-lookup"><span data-stu-id="665b3-258">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="665b3-259">`<input>`alteração</span><span class="sxs-lookup"><span data-stu-id="665b3-259">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="665b3-260">Teclado</span><span class="sxs-lookup"><span data-stu-id="665b3-260">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="665b3-261">Mouse</span><span class="sxs-lookup"><span data-stu-id="665b3-261">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="665b3-262">Ponteiro do mouse</span><span class="sxs-lookup"><span data-stu-id="665b3-262">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="665b3-263">Roda do mouse</span><span class="sxs-lookup"><span data-stu-id="665b3-263">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="665b3-264">Progresso</span><span class="sxs-lookup"><span data-stu-id="665b3-264">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="665b3-265">Toque</span><span class="sxs-lookup"><span data-stu-id="665b3-265">Touch</span></span> | <span data-ttu-id="665b3-266">`UITouchEventArgs`&ndash; representaumúnicopontodecontatoemum`UITouchPoint` dispositivo sensível ao toque.</span><span class="sxs-lookup"><span data-stu-id="665b3-266">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="665b3-267">Para obter informações sobre as propriedades e o comportamento de manipulação de eventos dos eventos na tabela anterior, consulte [classes EventArgs na origem de referência](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span><span class="sxs-lookup"><span data-stu-id="665b3-267">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="665b3-268">Expressões lambda</span><span class="sxs-lookup"><span data-stu-id="665b3-268">Lambda expressions</span></span>

<span data-ttu-id="665b3-269">As expressões lambda também podem ser usadas:</span><span class="sxs-lookup"><span data-stu-id="665b3-269">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="665b3-270">Geralmente, é conveniente fechar valores adicionais, como ao iterar em um conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="665b3-270">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="665b3-271">O exemplo a seguir cria três botões, cada um dos `UpdateHeading` quais chamadas passando um argumento`UIMouseEventArgs`de evento () e seu`buttonNumber`número de botão () quando selecionado na interface do usuário:</span><span class="sxs-lookup"><span data-stu-id="665b3-271">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="665b3-272">**Não** use a variável de loop (`i`) em um `for` loop diretamente em uma expressão lambda.</span><span class="sxs-lookup"><span data-stu-id="665b3-272">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="665b3-273">Caso contrário, a mesma variável é usada por todas as `i`expressões lambda, fazendo com que o valor seja o mesmo em todos os lambdas.</span><span class="sxs-lookup"><span data-stu-id="665b3-273">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="665b3-274">Sempre Capture seu valor em uma variável local (`buttonNumber` no exemplo anterior) e use-o.</span><span class="sxs-lookup"><span data-stu-id="665b3-274">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="665b3-275">EventCallback</span><span class="sxs-lookup"><span data-stu-id="665b3-275">EventCallback</span></span>

<span data-ttu-id="665b3-276">Um cenário comum com componentes aninhados é o desejo de executar o método de um componente pai quando ocorre&mdash;um evento de componente filho, por exemplo, quando um `onclick` evento ocorre no filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-276">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="665b3-277">Para expor eventos entre componentes, use um `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="665b3-277">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="665b3-278">Um componente pai pode atribuir um método de retorno de chamada a um `EventCallback`componente filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-278">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="665b3-279">O `ChildComponent` no aplicativo de exemplo demonstra como o `onclick` manipulador de um botão é configurado para receber um `EventCallback` delegado de exemplo `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="665b3-279">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="665b3-280">O `EventCallback` é digitado `UIMouseEventArgs`com, que é apropriado para `onclick` um evento de um dispositivo periférico:</span><span class="sxs-lookup"><span data-stu-id="665b3-280">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="665b3-281">O `ParentComponent` define o `EventCallback<T>` filho como seu `ShowMessage` método:</span><span class="sxs-lookup"><span data-stu-id="665b3-281">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="665b3-282">Quando o botão estiver selecionado no `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="665b3-282">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="665b3-283">O `ParentComponent`métodoéchamado `ShowMessage` .</span><span class="sxs-lookup"><span data-stu-id="665b3-283">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="665b3-284">`messageText`é atualizado e exibido no `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="665b3-284">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="665b3-285">Uma chamada para `StateHasChanged` não é necessária no método do retorno de`ShowMessage`chamada ().</span><span class="sxs-lookup"><span data-stu-id="665b3-285">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="665b3-286">`StateHasChanged`é chamado automaticamente para renderizar novamente `ParentComponent`o, assim como eventos filho, rerenderização de componente em manipuladores de eventos que são executados dentro do filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-286">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="665b3-287">`EventCallback`e `EventCallback<T>` permitir delegados assíncronos.</span><span class="sxs-lookup"><span data-stu-id="665b3-287">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="665b3-288">`EventCallback<T>`é fortemente tipado e requer um tipo de argumento específico.</span><span class="sxs-lookup"><span data-stu-id="665b3-288">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="665b3-289">`EventCallback`está com tipo fraco e permite qualquer tipo de argumento.</span><span class="sxs-lookup"><span data-stu-id="665b3-289">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="665b3-290">Invocar `EventCallback` um `EventCallback<T>` ou `InvokeAsync` com e aguardar <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="665b3-290">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="665b3-291">Use `EventCallback` e`EventCallback<T>` para manipulação de eventos e parâmetros de componente de associação.</span><span class="sxs-lookup"><span data-stu-id="665b3-291">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="665b3-292">Prefira o tipo fortemente `EventCallback<T>` tipado `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="665b3-292">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="665b3-293">`EventCallback<T>`fornece melhores comentários de erro para os usuários do componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-293">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="665b3-294">Semelhante a outros manipuladores de eventos de interface do usuário, especificar o parâmetro de evento é opcional.</span><span class="sxs-lookup"><span data-stu-id="665b3-294">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="665b3-295">Use `EventCallback` quando não houver valor passado para o retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="665b3-295">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="665b3-296">Capturar referências a componentes</span><span class="sxs-lookup"><span data-stu-id="665b3-296">Capture references to components</span></span>

<span data-ttu-id="665b3-297">As `Show` referências de componente fornecem uma maneira de fazer referência a uma instância de componente para que você possa emitir comandos para essa `Reset`instância, como ou.</span><span class="sxs-lookup"><span data-stu-id="665b3-297">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="665b3-298">Para capturar uma referência de componente, adicione [@ref](xref:mvc/views/razor#ref) um atributo ao componente filho e, em seguida, defina um campo com o mesmo nome e o mesmo tipo que o componente filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-298">To capture a component reference, add a [@ref](xref:mvc/views/razor#ref) attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="665b3-299">Quando o componente é renderizado, `loginDialog` o campo é populado com a instância de `MyLoginDialog` componente filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-299">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="665b3-300">Em seguida, você pode invocar os métodos .NET na instância do componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-300">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="665b3-301">A `loginDialog` variável é populada apenas depois que o componente é renderizado e `MyLoginDialog` sua saída inclui o elemento.</span><span class="sxs-lookup"><span data-stu-id="665b3-301">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="665b3-302">Até esse ponto, não há nada a fazer referência.</span><span class="sxs-lookup"><span data-stu-id="665b3-302">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="665b3-303">Para manipular referências de componentes após a conclusão da renderização do componente, `OnAfterRenderAsync` use `OnAfterRender` os métodos ou.</span><span class="sxs-lookup"><span data-stu-id="665b3-303">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="665b3-304">Embora a captura de referências de componente use uma sintaxe semelhante à [captura de referências de elemento](xref:blazor/javascript-interop#capture-references-to-elements), ela não é um recurso de interoperabilidade do [JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="665b3-304">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="665b3-305">As referências de componente não são passadas para o código&mdash;JavaScript que são usadas apenas no código .net.</span><span class="sxs-lookup"><span data-stu-id="665b3-305">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="665b3-306">Não use referências de componente para converter o estado dos componentes filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-306">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="665b3-307">Em vez disso, use parâmetros declarativos normais para passar dados para componentes filho.</span><span class="sxs-lookup"><span data-stu-id="665b3-307">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="665b3-308">O uso de parâmetros declarativos normais resulta em componentes filho que são reprocessados nos horários corretos automaticamente.</span><span class="sxs-lookup"><span data-stu-id="665b3-308">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="665b3-309">Use \@a chave para controlar a preservação de elementos e componentes</span><span class="sxs-lookup"><span data-stu-id="665b3-309">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="665b3-310">Ao renderizar uma lista de elementos ou componentes e, subsequentemente, os elementos ou componentes são alterados, o algoritmo de diferenciação do mais claro deve decidir quais elementos ou componentes anteriores podem ser mantidos e como os objetos de modelo devem ser mapeados para eles.</span><span class="sxs-lookup"><span data-stu-id="665b3-310">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="665b3-311">Normalmente, esse processo é automático e pode ser ignorado, mas há casos em que você talvez queira controlar o processo.</span><span class="sxs-lookup"><span data-stu-id="665b3-311">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="665b3-312">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="665b3-312">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="665b3-313">O conteúdo da `People` coleção pode ser alterado com entradas inseridas, excluídas ou reordenadas.</span><span class="sxs-lookup"><span data-stu-id="665b3-313">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="665b3-314">Quando o componente é rerenderizado, `<DetailsEditor>` o componente pode ser alterado para `Details` receber valores de parâmetro diferentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-314">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="665b3-315">Isso pode causar um reprocessamento mais complexo do que o esperado.</span><span class="sxs-lookup"><span data-stu-id="665b3-315">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="665b3-316">Em alguns casos, a rerenderização pode levar a diferenças de comportamento visíveis, como o foco de elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="665b3-316">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="665b3-317">O processo de mapeamento pode ser controlado com `@key` o atributo de diretiva.</span><span class="sxs-lookup"><span data-stu-id="665b3-317">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="665b3-318">`@key`faz com que o algoritmo diff garanta a preservação de elementos ou componentes com base no valor da chave:</span><span class="sxs-lookup"><span data-stu-id="665b3-318">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="665b3-319">Quando a `People` coleção é alterada, o algoritmo diff mantém a associação entre `<DetailsEditor>` instâncias e `person` instâncias:</span><span class="sxs-lookup"><span data-stu-id="665b3-319">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="665b3-320">Se um `Person` for excluído `People` da lista, somente a instância correspondente `<DetailsEditor>` será removida da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-320">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="665b3-321">Outras instâncias permanecem inalteradas.</span><span class="sxs-lookup"><span data-stu-id="665b3-321">Other instances are left unchanged.</span></span>
* <span data-ttu-id="665b3-322">Se um `Person` for inserido em alguma posição na lista, uma nova `<DetailsEditor>` instância será inserida na posição correspondente.</span><span class="sxs-lookup"><span data-stu-id="665b3-322">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="665b3-323">Outras instâncias permanecem inalteradas.</span><span class="sxs-lookup"><span data-stu-id="665b3-323">Other instances are left unchanged.</span></span>
* <span data-ttu-id="665b3-324">Se `Person` as entradas forem reordenadas, as `<DetailsEditor>` instâncias correspondentes serão preservadas e reordenadas na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-324">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="665b3-325">Em alguns cenários, o uso `@key` de minimiza a complexidade da rerenderização e evita possíveis problemas com partes com estado da alteração do dom, como a posição do foco.</span><span class="sxs-lookup"><span data-stu-id="665b3-325">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="665b3-326">As chaves são locais para cada elemento ou componente de contêiner.</span><span class="sxs-lookup"><span data-stu-id="665b3-326">Keys are local to each container element or component.</span></span> <span data-ttu-id="665b3-327">As chaves não são comparadas globalmente ao longo do documento.</span><span class="sxs-lookup"><span data-stu-id="665b3-327">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="665b3-328">Quando usar \@a chave</span><span class="sxs-lookup"><span data-stu-id="665b3-328">When to use \@key</span></span>

<span data-ttu-id="665b3-329">Normalmente, faz sentido usar `@key` sempre que uma lista é renderizada (por exemplo, em um `@foreach` bloco) e um valor adequado existe para definir o. `@key`</span><span class="sxs-lookup"><span data-stu-id="665b3-329">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="665b3-330">Você também pode usar `@key` o para evitar que o mais incrivelmente de preservar uma subárvore de elementos ou componentes quando um objeto for alterado:</span><span class="sxs-lookup"><span data-stu-id="665b3-330">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="665b3-331">Se `@currentPerson` forem alteradas `@key` , a diretiva de atributo forçará o mais `<div>` incrivelmente a descartar todo e seus descendentes e recriar a subárvore dentro da interface do usuário com novos elementos e componentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-331">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="665b3-332">Isso pode ser útil se você precisar garantir que nenhum estado da interface do usuário seja `@currentPerson` preservado quando houver alterações.</span><span class="sxs-lookup"><span data-stu-id="665b3-332">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="665b3-333">Quando não usar \@a chave</span><span class="sxs-lookup"><span data-stu-id="665b3-333">When not to use \@key</span></span>

<span data-ttu-id="665b3-334">Há um custo de desempenho ao comparar com `@key`.</span><span class="sxs-lookup"><span data-stu-id="665b3-334">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="665b3-335">O custo de desempenho não é grande, mas `@key` só especifica se controlar as regras de preservação de elementos ou componentes beneficiam o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="665b3-335">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="665b3-336">Mesmo que `@key` não seja usado, o mais grande preserva o elemento filho e as instâncias de componente o máximo possível.</span><span class="sxs-lookup"><span data-stu-id="665b3-336">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="665b3-337">A única vantagem de usar `@key` o é o controle sobre *como* as instâncias de modelo são mapeadas para as instâncias de componente preservadas, em vez do algoritmo diff, selecionando o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="665b3-337">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="665b3-338">Quais valores usar para \@a chave</span><span class="sxs-lookup"><span data-stu-id="665b3-338">What values to use for \@key</span></span>

<span data-ttu-id="665b3-339">Geralmente, faz sentido fornecer um dos seguintes tipos de valor para `@key`:</span><span class="sxs-lookup"><span data-stu-id="665b3-339">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="665b3-340">Instâncias de objeto de modelo (por exemplo `Person` , uma instância como no exemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="665b3-340">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="665b3-341">Isso garante a preservação com base na igualdade de referência de objeto.</span><span class="sxs-lookup"><span data-stu-id="665b3-341">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="665b3-342">Identificadores exclusivos (por exemplo, valores de chave primária do `int`tipo `string`, ou `Guid`).</span><span class="sxs-lookup"><span data-stu-id="665b3-342">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="665b3-343">Verifique se os valores usados `@key` para não conflitam.</span><span class="sxs-lookup"><span data-stu-id="665b3-343">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="665b3-344">Se os valores conflitantes forem detectados no mesmo elemento pai, o mais velho lançará uma exceção porque não pode mapear determinísticamente elementos ou componentes antigos para novos elementos ou componentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-344">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="665b3-345">Use apenas valores distintos, como instâncias de objeto ou valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="665b3-345">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="665b3-346">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="665b3-346">Lifecycle methods</span></span>

<span data-ttu-id="665b3-347">`OnInitAsync`e `OnInit` execute o código para inicializar o componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-347">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="665b3-348">Para executar uma operação assíncrona, `OnInitAsync` use e `await` a palavra-chave na operação:</span><span class="sxs-lookup"><span data-stu-id="665b3-348">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="665b3-349">Para uma operação síncrona, use `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="665b3-349">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="665b3-350">`OnParametersSetAsync`e `OnParametersSet` são chamados quando um componente recebe parâmetros de seu pai e os valores são atribuídos às propriedades.</span><span class="sxs-lookup"><span data-stu-id="665b3-350">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="665b3-351">Esses métodos são executados após a inicialização do componente e cada vez que o componente é renderizado:</span><span class="sxs-lookup"><span data-stu-id="665b3-351">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="665b3-352">`OnAfterRenderAsync`e `OnAfterRender` são chamados após a conclusão da renderização de um componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-352">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="665b3-353">Referências de elemento e componente são preenchidas neste ponto.</span><span class="sxs-lookup"><span data-stu-id="665b3-353">Element and component references are populated at this point.</span></span> <span data-ttu-id="665b3-354">Use este estágio para executar etapas de inicialização adicionais usando o conteúdo renderizado, como a ativação de bibliotecas JavaScript de terceiros que operam nos elementos DOM renderizados.</span><span class="sxs-lookup"><span data-stu-id="665b3-354">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="665b3-355">Tratar ações assíncronas incompletas no processamento</span><span class="sxs-lookup"><span data-stu-id="665b3-355">Handle incomplete async actions at render</span></span>

<span data-ttu-id="665b3-356">Ações assíncronas executadas em eventos de ciclo de vida podem não ter sido concluídas antes que o componente seja renderizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-356">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="665b3-357">Os objetos podem `null` ser ou preenchidos incompletamente com dados enquanto o método de ciclo de vida está em execução.</span><span class="sxs-lookup"><span data-stu-id="665b3-357">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="665b3-358">Forneça a lógica de renderização para confirmar que os objetos são inicializados.</span><span class="sxs-lookup"><span data-stu-id="665b3-358">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="665b3-359">Renderizar elementos de interface do usuário de espaço reservado (por exemplo, uma `null`mensagem de carregamento) enquanto objetos são.</span><span class="sxs-lookup"><span data-stu-id="665b3-359">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="665b3-360">No componente dos modelos mais claros, `OnInitAsync` é substituído para Asychronously receber dados de previsão (`forecasts`). `FetchData`</span><span class="sxs-lookup"><span data-stu-id="665b3-360">In the `FetchData` component of the Blazor templates, `OnInitAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="665b3-361">Quando `forecasts` é`null`, uma mensagem de carregamento é exibida para o usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-361">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="665b3-362">Depois que `Task` o retornado `OnInitAsync` por for concluído, o componente será rerenderizado com o estado atualizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-362">After the `Task` returned by `OnInitAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="665b3-363">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-363">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="665b3-364">Executar código antes que os parâmetros sejam definidos</span><span class="sxs-lookup"><span data-stu-id="665b3-364">Execute code before parameters are set</span></span>

<span data-ttu-id="665b3-365">`SetParameters`pode ser substituído para executar o código antes de os parâmetros serem definidos:</span><span class="sxs-lookup"><span data-stu-id="665b3-365">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="665b3-366">Se `base.SetParameters` não for invocado, o código personalizado poderá interpretar o valor dos parâmetros de entrada de qualquer forma necessária.</span><span class="sxs-lookup"><span data-stu-id="665b3-366">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="665b3-367">Por exemplo, os parâmetros de entrada não precisam ser atribuídos às propriedades na classe.</span><span class="sxs-lookup"><span data-stu-id="665b3-367">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="665b3-368">Suprimir a atualização da interface do usuário</span><span class="sxs-lookup"><span data-stu-id="665b3-368">Suppress refreshing of the UI</span></span>

<span data-ttu-id="665b3-369">`ShouldRender`pode ser substituído para suprimir a atualização da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-369">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="665b3-370">Se a implementação retornar `true`, a interface do usuário será atualizada.</span><span class="sxs-lookup"><span data-stu-id="665b3-370">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="665b3-371">Mesmo se `ShouldRender` for substituído, o componente sempre será renderizado inicialmente.</span><span class="sxs-lookup"><span data-stu-id="665b3-371">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="665b3-372">Descarte de componentes com IDisposable</span><span class="sxs-lookup"><span data-stu-id="665b3-372">Component disposal with IDisposable</span></span>

<span data-ttu-id="665b3-373">Se um componente implementa <xref:System.IDisposable>, o [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) é chamado quando o componente é removido da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-373">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="665b3-374">O componente a seguir `@implements IDisposable` usa o `Dispose` e o método:</span><span class="sxs-lookup"><span data-stu-id="665b3-374">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="665b3-375">Roteamento</span><span class="sxs-lookup"><span data-stu-id="665b3-375">Routing</span></span>

<span data-ttu-id="665b3-376">O roteamento no mais fácil é obtido fornecendo um modelo de rota para cada componente acessível no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="665b3-376">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="665b3-377">Quando um arquivo Razor com uma `@page` diretiva é compilado, a classe gerada recebe um <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> especificando o modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="665b3-377">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="665b3-378">Em tempo de execução, o roteador procura classes de componentes `RouteAttribute` com um e renderiza qualquer componente que tenha um modelo de rota que corresponda à URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="665b3-378">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="665b3-379">Vários modelos de rota podem ser aplicados a um componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-379">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="665b3-380">O componente a seguir responde a solicitações `/BlazorRoute` para `/DifferentBlazorRoute`o e o:</span><span class="sxs-lookup"><span data-stu-id="665b3-380">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="665b3-381">Parâmetros de rota</span><span class="sxs-lookup"><span data-stu-id="665b3-381">Route parameters</span></span>

<span data-ttu-id="665b3-382">Os componentes podem receber parâmetros de rota do modelo de rota fornecido `@page` na diretiva.</span><span class="sxs-lookup"><span data-stu-id="665b3-382">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="665b3-383">O roteador usa parâmetros de rota para preencher os parâmetros de componente correspondentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-383">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="665b3-384">*Componente de parâmetro de rota*:</span><span class="sxs-lookup"><span data-stu-id="665b3-384">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="665b3-385">Não há suporte para parâmetros opcionais `@page` , portanto, duas diretivas são aplicadas no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="665b3-385">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="665b3-386">O primeiro permite a navegação para o componente sem um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="665b3-386">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="665b3-387">A segunda `@page` diretiva usa o `{text}` parâmetro de rota e atribui o valor à `Text` propriedade.</span><span class="sxs-lookup"><span data-stu-id="665b3-387">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="665b3-388">Herança de classe base para uma experiência de "code-behind"</span><span class="sxs-lookup"><span data-stu-id="665b3-388">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="665b3-389">Os arquivos de componente misturam C# o código de marcação e processamento HTML no mesmo arquivo.</span><span class="sxs-lookup"><span data-stu-id="665b3-389">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="665b3-390">A `@inherits` diretiva pode ser usada para fornecer aos aplicativos mais incrivelmente uma experiência de "code-behind" que separa a marcação de componente do código de processamento.</span><span class="sxs-lookup"><span data-stu-id="665b3-390">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="665b3-391">O [aplicativo de exemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) mostra como um componente pode herdar uma classe `BlazorRocksBase`base,, para fornecer as propriedades e os métodos do componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-391">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="665b3-392">*Páginas/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="665b3-392">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="665b3-393">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="665b3-393">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="665b3-394">A classe base deve derivar `ComponentBase`de.</span><span class="sxs-lookup"><span data-stu-id="665b3-394">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="665b3-395">Importar componentes</span><span class="sxs-lookup"><span data-stu-id="665b3-395">Import components</span></span>

<span data-ttu-id="665b3-396">O namespace de um componente criado com o Razor se baseia em:</span><span class="sxs-lookup"><span data-stu-id="665b3-396">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="665b3-397">O projeto `RootNamespace`.</span><span class="sxs-lookup"><span data-stu-id="665b3-397">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="665b3-398">O caminho da raiz do projeto para o componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-398">The path from the project root to the component.</span></span> <span data-ttu-id="665b3-399">Por exemplo, `ComponentsSample/Pages/Index.razor` está no namespace. `ComponentsSample.Pages`</span><span class="sxs-lookup"><span data-stu-id="665b3-399">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="665b3-400">Os componentes C# seguem regras de associação de nome.</span><span class="sxs-lookup"><span data-stu-id="665b3-400">Components follow C# name binding rules.</span></span> <span data-ttu-id="665b3-401">No caso de *index. Razor*, todos os componentes na mesma pasta, *páginas*e a pasta pai, *ComponentsSample*, estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="665b3-401">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="665b3-402">Os componentes definidos em um namespace diferente podem ser trazidos para o escopo usando a diretiva [ \@using](xref:mvc/views/razor#using) do Razor.</span><span class="sxs-lookup"><span data-stu-id="665b3-402">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="665b3-403">Se outro componente, `NavMenu.razor`, existir na pasta `ComponentsSample/Shared/`, o `Index.razor` componente poderá ser usado com a seguinte `@using` instrução:</span><span class="sxs-lookup"><span data-stu-id="665b3-403">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="665b3-404">Os componentes também podem ser referenciados usando seus nomes totalmente qualificados, o que elimina a [ \@](xref:mvc/views/razor#using) necessidade da diretiva de uso:</span><span class="sxs-lookup"><span data-stu-id="665b3-404">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="665b3-405">Não `global::` há suporte para a qualificação.</span><span class="sxs-lookup"><span data-stu-id="665b3-405">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="665b3-406">Não há suporte para a `using` importação de componentes com instruções `@using Foo = Bar`com alias (por exemplo,).</span><span class="sxs-lookup"><span data-stu-id="665b3-406">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="665b3-407">Não há suporte para nomes parcialmente qualificados.</span><span class="sxs-lookup"><span data-stu-id="665b3-407">Partially qualified names aren't supported.</span></span> <span data-ttu-id="665b3-408">Por exemplo, não `@using ComponentsSample` há suporte `NavMenu.razor` para `<Shared.NavMenu></Shared.NavMenu>` adicionar e referenciar com.</span><span class="sxs-lookup"><span data-stu-id="665b3-408">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="665b3-409">Atributos de elemento HTML condicional</span><span class="sxs-lookup"><span data-stu-id="665b3-409">Conditional HTML element attributes</span></span>

<span data-ttu-id="665b3-410">Os atributos de elemento HTML são processados condicionalmente com base no valor do .NET.</span><span class="sxs-lookup"><span data-stu-id="665b3-410">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="665b3-411">Se o valor for `false` ou `null`, o atributo não será renderizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-411">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="665b3-412">Se o valor é `true`, o atributo é processado minimizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-412">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="665b3-413">No exemplo a seguir, `IsCompleted` determina se `checked` é renderizado na marcação do elemento:</span><span class="sxs-lookup"><span data-stu-id="665b3-413">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="665b3-414">Se `IsCompleted` for`true`, a caixa de seleção será renderizada como:</span><span class="sxs-lookup"><span data-stu-id="665b3-414">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="665b3-415">Se `IsCompleted` for`false`, a caixa de seleção será renderizada como:</span><span class="sxs-lookup"><span data-stu-id="665b3-415">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="665b3-416">Para obter mais informações, consulte <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="665b3-416">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="665b3-417">HTML bruto</span><span class="sxs-lookup"><span data-stu-id="665b3-417">Raw HTML</span></span>

<span data-ttu-id="665b3-418">Normalmente, as cadeias de caracteres são renderizadas usando nós de texto DOM, o que significa que qualquer marcação que ela possa conter será ignorada e tratada como texto literal.</span><span class="sxs-lookup"><span data-stu-id="665b3-418">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="665b3-419">Para renderizar HTML bruto, empacote o conteúdo HTML em `MarkupString` um valor.</span><span class="sxs-lookup"><span data-stu-id="665b3-419">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="665b3-420">O valor é analisado como HTML ou SVG e inserido no DOM.</span><span class="sxs-lookup"><span data-stu-id="665b3-420">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="665b3-421">O processamento de HTML bruto construído a partir de qualquer fonte não confiável é um **risco à segurança** e deve ser evitado!</span><span class="sxs-lookup"><span data-stu-id="665b3-421">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="665b3-422">O exemplo a seguir mostra como `MarkupString` usar o tipo para adicionar um bloco de conteúdo HTML estático à saída renderizada de um componente:</span><span class="sxs-lookup"><span data-stu-id="665b3-422">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="665b3-423">Componentes de modelo</span><span class="sxs-lookup"><span data-stu-id="665b3-423">Templated components</span></span>

<span data-ttu-id="665b3-424">Componentes modelo são componentes que aceitam um ou mais modelos de interface do usuário como parâmetros, que podem ser usados como parte da lógica de renderização do componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-424">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="665b3-425">Os componentes de modelo permitem que você crie componentes de nível superior que são mais reutilizáveis do que os componentes normais.</span><span class="sxs-lookup"><span data-stu-id="665b3-425">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="665b3-426">Alguns exemplos incluem:</span><span class="sxs-lookup"><span data-stu-id="665b3-426">A couple of examples include:</span></span>

* <span data-ttu-id="665b3-427">Um componente de tabela que permite que um usuário especifique modelos para o cabeçalho, as linhas e o rodapé da tabela.</span><span class="sxs-lookup"><span data-stu-id="665b3-427">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="665b3-428">Um componente de lista que permite que um usuário especifique um modelo para renderizar itens em uma lista.</span><span class="sxs-lookup"><span data-stu-id="665b3-428">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="665b3-429">Parâmetros de modelo</span><span class="sxs-lookup"><span data-stu-id="665b3-429">Template parameters</span></span>

<span data-ttu-id="665b3-430">Um componente modelo é definido especificando um ou mais parâmetros de componente do tipo `RenderFragment` ou. `RenderFragment<T>`</span><span class="sxs-lookup"><span data-stu-id="665b3-430">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="665b3-431">Um fragmento de renderização representa um segmento de interface do usuário a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="665b3-431">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="665b3-432">`RenderFragment<T>`usa um parâmetro de tipo que pode ser especificado quando o fragmento de renderização é invocado.</span><span class="sxs-lookup"><span data-stu-id="665b3-432">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="665b3-433">`TableTemplate`componente</span><span class="sxs-lookup"><span data-stu-id="665b3-433">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="665b3-434">Ao usar um componente modelo, os parâmetros do modelo podem ser especificados usando elementos filho que correspondem aos nomes dos parâmetros (`TableHeader` e `RowTemplate` no exemplo a seguir):</span><span class="sxs-lookup"><span data-stu-id="665b3-434">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="665b3-435">Parâmetros de contexto de modelo</span><span class="sxs-lookup"><span data-stu-id="665b3-435">Template context parameters</span></span>

<span data-ttu-id="665b3-436">Argumentos de componente do `RenderFragment<T>` tipo passado como elementos têm um parâmetro implícito `context` chamado (por exemplo, `@context.PetId`do exemplo de código anterior), mas você pode alterar o nome do parâmetro `Context` usando o atributo no filho elementos.</span><span class="sxs-lookup"><span data-stu-id="665b3-436">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="665b3-437">No exemplo a seguir, o `RowTemplate` atributo do `Context` elemento Especifica o `pet` parâmetro:</span><span class="sxs-lookup"><span data-stu-id="665b3-437">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="665b3-438">Como alternativa, você pode especificar o `Context` atributo no elemento Component.</span><span class="sxs-lookup"><span data-stu-id="665b3-438">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="665b3-439">O atributo `Context` especificado se aplica a todos os parâmetros de modelo especificados.</span><span class="sxs-lookup"><span data-stu-id="665b3-439">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="665b3-440">Isso pode ser útil quando você deseja especificar o nome do parâmetro de conteúdo para conteúdo filho implícito (sem qualquer elemento filho de disposição).</span><span class="sxs-lookup"><span data-stu-id="665b3-440">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="665b3-441">No exemplo a seguir, o `Context` atributo aparece `TableTemplate` no elemento e se aplica a todos os parâmetros de modelo:</span><span class="sxs-lookup"><span data-stu-id="665b3-441">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="665b3-442">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="665b3-442">Generic-typed components</span></span>

<span data-ttu-id="665b3-443">Os componentes modelo são geralmente digitados genericamente.</span><span class="sxs-lookup"><span data-stu-id="665b3-443">Templated components are often generically typed.</span></span> <span data-ttu-id="665b3-444">Por exemplo, um componente `ListViewTemplate` genérico pode ser usado para renderizar `IEnumerable<T>` valores.</span><span class="sxs-lookup"><span data-stu-id="665b3-444">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="665b3-445">Para definir um componente genérico, use a `@typeparam` diretiva para especificar parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="665b3-445">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="665b3-446">Ao usar componentes de tipos genéricos, o parâmetro de tipo é inferido, se possível:</span><span class="sxs-lookup"><span data-stu-id="665b3-446">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="665b3-447">Caso contrário, o parâmetro de tipo deve ser especificado explicitamente usando um atributo que corresponda ao nome do parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="665b3-447">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="665b3-448">No exemplo a seguir, `TItem="Pet"` especifica o tipo:</span><span class="sxs-lookup"><span data-stu-id="665b3-448">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="665b3-449">Valores e parâmetros em cascata</span><span class="sxs-lookup"><span data-stu-id="665b3-449">Cascading values and parameters</span></span>

<span data-ttu-id="665b3-450">Em alguns cenários, é inconveniente fluir dados de um componente ancestral para um componente descendente usando [parâmetros de componente](#component-parameters), especialmente quando há várias camadas de componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-450">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="665b3-451">Valores e parâmetros em cascata resolvem esse problema fornecendo uma maneira conveniente para um componente ancestral fornecer um valor para todos os seus componentes descendentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-451">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="665b3-452">Valores e parâmetros em cascata também fornecem uma abordagem para que os componentes sejam coordenados.</span><span class="sxs-lookup"><span data-stu-id="665b3-452">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="665b3-453">Exemplo de tema</span><span class="sxs-lookup"><span data-stu-id="665b3-453">Theme example</span></span>

<span data-ttu-id="665b3-454">No exemplo a seguir do aplicativo de exemplo, a `ThemeInfo` classe especifica as informações do tema para fluir para baixo na hierarquia do componente para que todos os botões de uma determinada parte do aplicativo compartilhem o mesmo estilo.</span><span class="sxs-lookup"><span data-stu-id="665b3-454">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="665b3-455">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="665b3-455">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="665b3-456">Um componente ancestral pode fornecer um valor em cascata usando o componente de valor em cascata.</span><span class="sxs-lookup"><span data-stu-id="665b3-456">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="665b3-457">O `CascadingValue` componente encapsula uma subárvore da hierarquia do componente e fornece um único valor para todos os componentes dentro dessa subárvore.</span><span class="sxs-lookup"><span data-stu-id="665b3-457">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="665b3-458">Por exemplo, o aplicativo de exemplo especifica informações de`ThemeInfo`tema () em um dos layouts do aplicativo como um parâmetro em cascata para todos os componentes que compõem o corpo `@Body` do layout da propriedade.</span><span class="sxs-lookup"><span data-stu-id="665b3-458">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="665b3-459">`ButtonClass`é atribuído um valor de `btn-success` no componente layout.</span><span class="sxs-lookup"><span data-stu-id="665b3-459">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="665b3-460">Qualquer componente descendente pode consumir essa propriedade por meio `ThemeInfo` do objeto em cascata.</span><span class="sxs-lookup"><span data-stu-id="665b3-460">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="665b3-461">`CascadingValuesParametersLayout`componente</span><span class="sxs-lookup"><span data-stu-id="665b3-461">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="665b3-462">Para fazer uso de valores em cascata, os componentes declaram parâmetros em `[CascadingParameter]` cascata usando o atributo ou com base em um valor de nome de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="665b3-462">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="665b3-463">A associação com um valor de nome de cadeia de caracteres será relevante se você tiver vários valores em cascata do mesmo tipo e precisar diferenciá-los na mesma subárvore.</span><span class="sxs-lookup"><span data-stu-id="665b3-463">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="665b3-464">Os valores em cascata são associados a parâmetros em cascata por tipo.</span><span class="sxs-lookup"><span data-stu-id="665b3-464">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="665b3-465">No aplicativo de exemplo, o `CascadingValuesParametersTheme` componente associa o `ThemeInfo` valor em cascata a um parâmetro em cascata.</span><span class="sxs-lookup"><span data-stu-id="665b3-465">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="665b3-466">O parâmetro é usado para definir a classe CSS para um dos botões exibidos pelo componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-466">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="665b3-467">`CascadingValuesParametersTheme`componente</span><span class="sxs-lookup"><span data-stu-id="665b3-467">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="665b3-468">Exemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="665b3-468">TabSet example</span></span>

<span data-ttu-id="665b3-469">Os parâmetros em cascata também permitem que os componentes colaborem na hierarquia do componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-469">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="665b3-470">Por exemplo, considere o exemplo de *TabSet* a seguir no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="665b3-470">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="665b3-471">O aplicativo de exemplo tem `ITab` uma interface que implementa as guias:</span><span class="sxs-lookup"><span data-stu-id="665b3-471">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="665b3-472">O `CascadingValuesParametersTabSet` componente usa o `TabSet` componente, que contém vários `Tab` componentes:</span><span class="sxs-lookup"><span data-stu-id="665b3-472">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="665b3-473">Os componentes `Tab` filho não são passados explicitamente como parâmetros `TabSet`para.</span><span class="sxs-lookup"><span data-stu-id="665b3-473">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="665b3-474">Em vez disso, `Tab` os componentes filho fazem parte do conteúdo filho `TabSet`do.</span><span class="sxs-lookup"><span data-stu-id="665b3-474">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="665b3-475">No entanto `TabSet` , o ainda precisa saber sobre `Tab` cada componente para que ele possa renderizar os cabeçalhos e a guia ativa. Para habilitar essa coordenação sem a necessidade de código adicional, `TabSet` o componente *pode fornecer a si mesmo como um valor em cascata* que é então coletado `Tab` pelos componentes descendentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-475">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="665b3-476">`TabSet`componente</span><span class="sxs-lookup"><span data-stu-id="665b3-476">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="665b3-477">Os componentes `Tab` descendentes capturam `TabSet` o que contém como um parâmetro em cascata `Tab` , de modo que os `TabSet` componentes se adicionam à coordenada e na qual a guia está ativa.</span><span class="sxs-lookup"><span data-stu-id="665b3-477">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="665b3-478">`Tab`componente</span><span class="sxs-lookup"><span data-stu-id="665b3-478">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="665b3-479">Modelos do Razor</span><span class="sxs-lookup"><span data-stu-id="665b3-479">Razor templates</span></span>

<span data-ttu-id="665b3-480">Os fragmentos de renderização podem ser definidos usando a sintaxe de modelo Razor.</span><span class="sxs-lookup"><span data-stu-id="665b3-480">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="665b3-481">Os modelos Razor são uma maneira de definir um trecho de interface do usuário e assumir o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="665b3-481">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="665b3-482">O exemplo a seguir ilustra como especificar `RenderFragment` e `RenderFragment<T>` valores e renderizar modelos diretamente em um componente.</span><span class="sxs-lookup"><span data-stu-id="665b3-482">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="665b3-483">Os fragmentos de renderização também podem ser passados como argumentos para [componentes de modelo](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="665b3-483">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="665b3-484">Saída renderizada do código anterior:</span><span class="sxs-lookup"><span data-stu-id="665b3-484">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="665b3-485">Lógica RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="665b3-485">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="665b3-486">`Microsoft.AspNetCore.Components.RenderTree`fornece métodos para manipular componentes e elementos, incluindo a criação manual de componentes C# no código.</span><span class="sxs-lookup"><span data-stu-id="665b3-486">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="665b3-487">O uso `RenderTreeBuilder` do para criar componentes é um cenário avançado.</span><span class="sxs-lookup"><span data-stu-id="665b3-487">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="665b3-488">Um componente malformado (por exemplo, uma marca de marcação não fechada) pode resultar em um comportamento indefinido.</span><span class="sxs-lookup"><span data-stu-id="665b3-488">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="665b3-489">Considere o seguinte `PetDetails` componente, que pode ser compilado manualmente em outro componente:</span><span class="sxs-lookup"><span data-stu-id="665b3-489">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="665b3-490">No exemplo a seguir, o loop no `CreateComponent` método gera três `PetDetails` componentes.</span><span class="sxs-lookup"><span data-stu-id="665b3-490">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="665b3-491">Ao chamar `RenderTreeBuilder` métodos para criar os componentes (`OpenComponent` e `AddAttribute`), os números de sequência são números de linha de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="665b3-491">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="665b3-492">O algoritmo de diferença mais grande do que se baseia nos números de sequência correspondentes a linhas distintas de código, não a invocações de chamada distintas.</span><span class="sxs-lookup"><span data-stu-id="665b3-492">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="665b3-493">Ao criar um componente com `RenderTreeBuilder` métodos, codifique os argumentos para números de sequência.</span><span class="sxs-lookup"><span data-stu-id="665b3-493">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="665b3-494">**O uso de um cálculo ou contador para gerar o número de sequência pode levar a um desempenho insatisfatório.**</span><span class="sxs-lookup"><span data-stu-id="665b3-494">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="665b3-495">Para obter mais informações, consulte os [números de sequência relacionados à seção números de linha de código e não ordem de execução](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="665b3-495">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="665b3-496">`BuiltContent`componente</span><span class="sxs-lookup"><span data-stu-id="665b3-496">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="665b3-497">Números de sequência se relacionam a números de linha de código e não a ordem de execução</span><span class="sxs-lookup"><span data-stu-id="665b3-497">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="665b3-498">`.razor` Arquivos mais poseriais são sempre compilados.</span><span class="sxs-lookup"><span data-stu-id="665b3-498">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="665b3-499">Isso é potencialmente uma grande vantagem para `.razor` o porque a etapa de compilação pode ser usada para injetar informações que melhoram o desempenho do aplicativo em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="665b3-499">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="665b3-500">Um exemplo importante desses aprimoramentos envolve *números de sequência*.</span><span class="sxs-lookup"><span data-stu-id="665b3-500">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="665b3-501">Os números de sequência indicam ao tempo de execução que as saídas vieram de quais linhas de código distintas e ordenadas.</span><span class="sxs-lookup"><span data-stu-id="665b3-501">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="665b3-502">O tempo de execução usa essas informações para gerar comparações de árvore eficientes em tempo linear, o que é muito mais rápido do que normalmente é possível para um algoritmo de comparação de árvore geral.</span><span class="sxs-lookup"><span data-stu-id="665b3-502">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="665b3-503">Considere o seguinte arquivo `.razor` simples:</span><span class="sxs-lookup"><span data-stu-id="665b3-503">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="665b3-504">O código anterior é compilado para algo semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="665b3-504">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="665b3-505">Quando o código é executado pela primeira vez, se `someFlag` for `true`, o Construtor receberá:</span><span class="sxs-lookup"><span data-stu-id="665b3-505">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="665b3-506">Sequência</span><span class="sxs-lookup"><span data-stu-id="665b3-506">Sequence</span></span> | <span data-ttu-id="665b3-507">Tipo</span><span class="sxs-lookup"><span data-stu-id="665b3-507">Type</span></span>      | <span data-ttu-id="665b3-508">Dados</span><span class="sxs-lookup"><span data-stu-id="665b3-508">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="665b3-509">0</span><span class="sxs-lookup"><span data-stu-id="665b3-509">0</span></span>        | <span data-ttu-id="665b3-510">Nó de texto</span><span class="sxs-lookup"><span data-stu-id="665b3-510">Text node</span></span> | <span data-ttu-id="665b3-511">First</span><span class="sxs-lookup"><span data-stu-id="665b3-511">First</span></span>  |
| <span data-ttu-id="665b3-512">1</span><span class="sxs-lookup"><span data-stu-id="665b3-512">1</span></span>        | <span data-ttu-id="665b3-513">Nó de texto</span><span class="sxs-lookup"><span data-stu-id="665b3-513">Text node</span></span> | <span data-ttu-id="665b3-514">Segundo</span><span class="sxs-lookup"><span data-stu-id="665b3-514">Second</span></span> |

<span data-ttu-id="665b3-515">Imagine que `someFlag` se `false`torna e a marcação é renderizada novamente.</span><span class="sxs-lookup"><span data-stu-id="665b3-515">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="665b3-516">Desta vez, o Construtor recebe:</span><span class="sxs-lookup"><span data-stu-id="665b3-516">This time, the builder receives:</span></span>

| <span data-ttu-id="665b3-517">Sequência</span><span class="sxs-lookup"><span data-stu-id="665b3-517">Sequence</span></span> | <span data-ttu-id="665b3-518">Tipo</span><span class="sxs-lookup"><span data-stu-id="665b3-518">Type</span></span>       | <span data-ttu-id="665b3-519">Dados</span><span class="sxs-lookup"><span data-stu-id="665b3-519">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="665b3-520">1</span><span class="sxs-lookup"><span data-stu-id="665b3-520">1</span></span>        | <span data-ttu-id="665b3-521">Nó de texto</span><span class="sxs-lookup"><span data-stu-id="665b3-521">Text node</span></span>  | <span data-ttu-id="665b3-522">Segundo</span><span class="sxs-lookup"><span data-stu-id="665b3-522">Second</span></span> |

<span data-ttu-id="665b3-523">Quando o tempo de execução executa uma comparação, ele vê que o item `0` na sequência foi removido e, portanto, gera o seguinte *script de edição*trivial:</span><span class="sxs-lookup"><span data-stu-id="665b3-523">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="665b3-524">Remova o primeiro nó de texto.</span><span class="sxs-lookup"><span data-stu-id="665b3-524">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="665b3-525">O que vai errado se você gerar números de sequência programaticamente</span><span class="sxs-lookup"><span data-stu-id="665b3-525">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="665b3-526">Imagine, em vez disso, que você escreveu a seguinte lógica do construtor de árvore de renderização:</span><span class="sxs-lookup"><span data-stu-id="665b3-526">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="665b3-527">Agora, a primeira saída é:</span><span class="sxs-lookup"><span data-stu-id="665b3-527">Now, the first output is:</span></span>

| <span data-ttu-id="665b3-528">Sequência</span><span class="sxs-lookup"><span data-stu-id="665b3-528">Sequence</span></span> | <span data-ttu-id="665b3-529">Tipo</span><span class="sxs-lookup"><span data-stu-id="665b3-529">Type</span></span>      | <span data-ttu-id="665b3-530">Dados</span><span class="sxs-lookup"><span data-stu-id="665b3-530">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="665b3-531">0</span><span class="sxs-lookup"><span data-stu-id="665b3-531">0</span></span>        | <span data-ttu-id="665b3-532">Nó de texto</span><span class="sxs-lookup"><span data-stu-id="665b3-532">Text node</span></span> | <span data-ttu-id="665b3-533">First</span><span class="sxs-lookup"><span data-stu-id="665b3-533">First</span></span>  |
| <span data-ttu-id="665b3-534">1</span><span class="sxs-lookup"><span data-stu-id="665b3-534">1</span></span>        | <span data-ttu-id="665b3-535">Nó de texto</span><span class="sxs-lookup"><span data-stu-id="665b3-535">Text node</span></span> | <span data-ttu-id="665b3-536">Segundo</span><span class="sxs-lookup"><span data-stu-id="665b3-536">Second</span></span> |

<span data-ttu-id="665b3-537">Esse resultado é idêntico ao caso anterior, portanto, não existem problemas negativos.</span><span class="sxs-lookup"><span data-stu-id="665b3-537">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="665b3-538">`someFlag`está `false` no segundo processamento e a saída é:</span><span class="sxs-lookup"><span data-stu-id="665b3-538">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="665b3-539">Sequência</span><span class="sxs-lookup"><span data-stu-id="665b3-539">Sequence</span></span> | <span data-ttu-id="665b3-540">Tipo</span><span class="sxs-lookup"><span data-stu-id="665b3-540">Type</span></span>      | <span data-ttu-id="665b3-541">Dados</span><span class="sxs-lookup"><span data-stu-id="665b3-541">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="665b3-542">0</span><span class="sxs-lookup"><span data-stu-id="665b3-542">0</span></span>        | <span data-ttu-id="665b3-543">Nó de texto</span><span class="sxs-lookup"><span data-stu-id="665b3-543">Text node</span></span> | <span data-ttu-id="665b3-544">Segundo</span><span class="sxs-lookup"><span data-stu-id="665b3-544">Second</span></span> |

<span data-ttu-id="665b3-545">Desta vez, o algoritmo diff vê que *duas* alterações ocorreram e o algoritmo gera o seguinte script de edição:</span><span class="sxs-lookup"><span data-stu-id="665b3-545">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="665b3-546">Altere o valor do primeiro nó de texto para `Second`.</span><span class="sxs-lookup"><span data-stu-id="665b3-546">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="665b3-547">Remova o segundo nó de texto.</span><span class="sxs-lookup"><span data-stu-id="665b3-547">Remove the second text node.</span></span>

<span data-ttu-id="665b3-548">A geração de números de sequência perdeu todas as informações úteis sobre onde `if/else` os branches e loops estavam presentes no código original.</span><span class="sxs-lookup"><span data-stu-id="665b3-548">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="665b3-549">Isso resulta em uma comparação **duas vezes mais longa** do que antes.</span><span class="sxs-lookup"><span data-stu-id="665b3-549">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="665b3-550">Esse é um exemplo trivial.</span><span class="sxs-lookup"><span data-stu-id="665b3-550">This is a trivial example.</span></span> <span data-ttu-id="665b3-551">Em casos mais realistas com estruturas complexas e profundamente aninhadas, e especialmente com loops, o custo de desempenho é mais grave.</span><span class="sxs-lookup"><span data-stu-id="665b3-551">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="665b3-552">Em vez de identificar imediatamente quais blocos de loop ou ramificações foram inseridos ou removidos, o algoritmo diff precisa recorrer profundamente nas árvores de renderização e geralmente criar scripts de edição muito mais, pois ele é informado indiretamente sobre como as estruturas antigas e novas relacionar entre si.</span><span class="sxs-lookup"><span data-stu-id="665b3-552">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="665b3-553">Diretrizes e conclusões</span><span class="sxs-lookup"><span data-stu-id="665b3-553">Guidance and conclusions</span></span>

* <span data-ttu-id="665b3-554">O desempenho do aplicativo será afetado se os números de sequência forem gerados dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="665b3-554">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="665b3-555">A estrutura não pode criar seus próprios números de sequência automaticamente em tempo de execução porque as informações necessárias não existem, a menos que sejam capturadas no momento da compilação.</span><span class="sxs-lookup"><span data-stu-id="665b3-555">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="665b3-556">Não grave blocos longos de lógica implementada `RenderTreeBuilder` manualmente.</span><span class="sxs-lookup"><span data-stu-id="665b3-556">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="665b3-557">Prefira `.razor` arquivos e permita que o compilador lide com os números de sequência.</span><span class="sxs-lookup"><span data-stu-id="665b3-557">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="665b3-558">Se os números de sequência forem codificados, o algoritmo diff só exigirá que os números de sequência aumentem de valor.</span><span class="sxs-lookup"><span data-stu-id="665b3-558">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="665b3-559">O valor inicial e as lacunas são irrelevantes.</span><span class="sxs-lookup"><span data-stu-id="665b3-559">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="665b3-560">Uma opção legítima é usar o número de linha de código como o número de sequência, ou começar de zero e aumentar por um ou centenas (ou qualquer intervalo preferencial).</span><span class="sxs-lookup"><span data-stu-id="665b3-560">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="665b3-561">O mais alto número de seqüências usa números de sequência, enquanto outras estruturas de interface do usuário de diferenciação de árvore não as usam.</span><span class="sxs-lookup"><span data-stu-id="665b3-561">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="665b3-562">A comparação é muito mais rápida quando os números de sequência são usados, e o mais vantajoso tem a vantagem de uma etapa de compilação que lida com números de `.razor` sequência automaticamente para desenvolvedores que criam arquivos.</span><span class="sxs-lookup"><span data-stu-id="665b3-562">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="665b3-563">Localização</span><span class="sxs-lookup"><span data-stu-id="665b3-563">Localization</span></span>

<span data-ttu-id="665b3-564">Os aplicativos mais no lado do servidor são localizados usando o [middleware de localização](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="665b3-564">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="665b3-565">O middleware seleciona a cultura apropriada para os usuários que solicitam recursos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="665b3-565">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="665b3-566">A cultura pode ser definida usando uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="665b3-566">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="665b3-567">Cookies</span><span class="sxs-lookup"><span data-stu-id="665b3-567">Cookies</span></span>](#cookies)
* [<span data-ttu-id="665b3-568">Fornecer interface do usuário para escolher a cultura</span><span class="sxs-lookup"><span data-stu-id="665b3-568">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="665b3-569">Para obter mais informações e exemplos, consulte <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="665b3-569">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="665b3-570">Cookies</span><span class="sxs-lookup"><span data-stu-id="665b3-570">Cookies</span></span>

<span data-ttu-id="665b3-571">Um cookie de cultura de localização pode persistir a cultura do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-571">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="665b3-572">O cookie é criado pelo `OnGet` método da página host do aplicativo (*pages/host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="665b3-572">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="665b3-573">O middleware de localização lê o cookie em solicitações subsequentes para definir a cultura do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-573">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="665b3-574">O uso de um cookie garante que a conexão WebSocket possa propagar corretamente a cultura.</span><span class="sxs-lookup"><span data-stu-id="665b3-574">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="665b3-575">Se os esquemas de localização forem baseados no caminho da URL ou na cadeia de caracteres de consulta, o esquema pode não ser capaz de trabalhar com WebSockets, portanto, falha ao persistir a cultura.</span><span class="sxs-lookup"><span data-stu-id="665b3-575">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="665b3-576">Portanto, o uso de um cookie de cultura de localização é a abordagem recomendada.</span><span class="sxs-lookup"><span data-stu-id="665b3-576">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="665b3-577">Qualquer técnica pode ser usada para atribuir uma cultura se a cultura persistir em um cookie de localização.</span><span class="sxs-lookup"><span data-stu-id="665b3-577">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="665b3-578">Se o aplicativo já tiver um esquema de localização estabelecido para ASP.NET Core do lado do servidor, continue a usar a infraestrutura de localização existente do aplicativo e defina o cookie de cultura de localização no esquema do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="665b3-578">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="665b3-579">O exemplo a seguir mostra como definir a cultura atual em um cookie que pode ser lido pelo middleware de localização.</span><span class="sxs-lookup"><span data-stu-id="665b3-579">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="665b3-580">Crie um arquivo *pages/host. cshtml. cs* com o seguinte conteúdo no aplicativo mais novo do servidor:</span><span class="sxs-lookup"><span data-stu-id="665b3-580">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="665b3-581">A localização é manipulada no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="665b3-581">Localization is handled in the app:</span></span>

1. <span data-ttu-id="665b3-582">O navegador envia uma solicitação HTTP inicial para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="665b3-582">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="665b3-583">A cultura é atribuída pelo middleware de localização.</span><span class="sxs-lookup"><span data-stu-id="665b3-583">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="665b3-584">O `OnGet` método em *_Host. cshtml. cs* persiste a cultura em um cookie como parte da resposta.</span><span class="sxs-lookup"><span data-stu-id="665b3-584">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="665b3-585">O navegador abre uma conexão WebSocket para criar uma sessão do lado do servidor mais incrivelmente interativa.</span><span class="sxs-lookup"><span data-stu-id="665b3-585">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="665b3-586">O middleware de localização lê o cookie e atribui a cultura.</span><span class="sxs-lookup"><span data-stu-id="665b3-586">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="665b3-587">A sessão mais incrivelmente no lado do servidor começa com a cultura correta.</span><span class="sxs-lookup"><span data-stu-id="665b3-587">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="665b3-588">Fornecer interface do usuário para escolher a cultura</span><span class="sxs-lookup"><span data-stu-id="665b3-588">Provide UI to choose the culture</span></span>

<span data-ttu-id="665b3-589">Para fornecer à interface do usuário a fim de permitir a seleção de uma cultura, é recomendável uma *abordagem baseada em* redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="665b3-589">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="665b3-590">O processo é semelhante ao que acontece em um aplicativo Web quando um usuário tenta acessar um recurso&mdash;seguro que o usuário é redirecionado para uma página de entrada e, em seguida, Redirecionado de volta para o recurso original.</span><span class="sxs-lookup"><span data-stu-id="665b3-590">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="665b3-591">O aplicativo persiste a cultura selecionada do usuário por meio de um redirecionamento para um controlador.</span><span class="sxs-lookup"><span data-stu-id="665b3-591">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="665b3-592">O controlador define a cultura selecionada do usuário em um cookie e redireciona o usuário de volta para o URI original.</span><span class="sxs-lookup"><span data-stu-id="665b3-592">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="665b3-593">Estabeleça um ponto de extremidade HTTP no servidor para definir a cultura selecionada do usuário em um cookie e execute o redirecionamento de volta para o URI original:</span><span class="sxs-lookup"><span data-stu-id="665b3-593">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="665b3-594">Use o `LocalRedirect` resultado da ação para evitar ataques de redirecionamento abertos.</span><span class="sxs-lookup"><span data-stu-id="665b3-594">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="665b3-595">Para obter mais informações, consulte <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="665b3-595">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="665b3-596">O componente a seguir mostra um exemplo de como executar o redirecionamento inicial quando o usuário seleciona uma cultura:</span><span class="sxs-lookup"><span data-stu-id="665b3-596">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject IUriHelper UriHelper

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(UIChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(UriHelper.GetAbsoluteUri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        UriHelper.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="665b3-597">Usar cenários de localização do .NET em aplicativos mais Incrivelmenteos</span><span class="sxs-lookup"><span data-stu-id="665b3-597">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="665b3-598">Nos aplicativos mais poseriais, os seguintes cenários de localização e globalização do .NET estão disponíveis:</span><span class="sxs-lookup"><span data-stu-id="665b3-598">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="665b3-599">. Sistema de recursos da rede</span><span class="sxs-lookup"><span data-stu-id="665b3-599">.NET's resources system</span></span>
* <span data-ttu-id="665b3-600">Formatação de número e data específicos da cultura</span><span class="sxs-lookup"><span data-stu-id="665b3-600">Culture-specific number and date formatting</span></span>

<span data-ttu-id="665b3-601">A funcionalidade de `@bind` mais de uma das mais recentes realiza a globalização com base na cultura atual do usuário.</span><span class="sxs-lookup"><span data-stu-id="665b3-601">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="665b3-602">Para obter mais informações, consulte a seção [ligação de dados](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="665b3-602">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="665b3-603">No momento, há suporte para um conjunto limitado de cenários de localização de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="665b3-603">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="665b3-604">`IStringLocalizer<>`tem *suporte* em aplicativos mais incrivelmenteos.</span><span class="sxs-lookup"><span data-stu-id="665b3-604">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="665b3-605">`IHtmlLocalizer<>`, `IViewLocalizer<>`, e a localização de anotações de dados são ASP.NET Core cenários MVC e **não têm suporte** em aplicativos mais incrivelmenteos.</span><span class="sxs-lookup"><span data-stu-id="665b3-605">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="665b3-606">Para obter mais informações, consulte <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="665b3-606">For more information, see <xref:fundamentals/localization>.</span></span>