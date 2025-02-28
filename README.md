


</head>

<body class="fullcontent">

<div id="quarto-content" class="page-columns page-rows-contents page-layout-article">

<main class="content" id="quarto-document-content">

<header id="title-block-header" class="quarto-title-block default">
<div class="quarto-title">
<h1 class="title">Bin Quality Report</h1>
</div>



<div class="quarto-title-meta">

    
  
    
  </div>
  


</header>


<div class="callout callout-style-default callout-tip callout-titled">
<div class="callout-header d-flex align-content-center">
<div class="callout-icon-container">
<i class="callout-icon"></i>
</div>
<div class="callout-title-container flex-fill">
Tip
</div>
</div>
<div class="callout-body-container callout-body">
<p><strong>Table of Contents</strong><br>
Below is the list of sections covered in this document.<br>
Click on any section to quickly navigate to it.</p>
<ol type="1">
<li><a href="#Library_and_Data_Import">Library and Data Import</a> </li>
<li><a href="#Overview">Overview</a> </li>
<li><a href="#completeness">completeness</a> </li>
<li><a href="#bins_completeness_scores_based_on_lineage">bins completeness scores based on lineage</a> </li>
<li><a href="#contamination">contamination</a> </li>
<li><a href="#bins_contamination_scores_based_on_lineage">bins contamination scores based on lineage</a> </li>
<li><a href="#bins_CG_content_based_on_lineage">Bins CG content based on lineage</a> </li>
<li><a href="#Bin_size">Bin size</a> </li>
<li><a href="#N50">N50</a> </li>
<li><a href="#Bin_Comparision">Bin comparision</a></li>
</ol>
</div>
</div>
<section id="Library_and_Data_Import" class="level4">
<h4 class="anchored" data-anchor-id="Library_and_Data_Import">Library and Data Import</h4>
<div class="cell">
<div class="sourceCode cell-code" id="cb1"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb1-1"><a href="#cb1-1" aria-hidden="true" tabindex="-1"></a><span class="co"># install libraries if they are not availible</span></span>
<span id="cb1-2"><a href="#cb1-2" aria-hidden="true" tabindex="-1"></a><span class="co"># required libraries</span></span>
<span id="cb1-3"><a href="#cb1-3" aria-hidden="true" tabindex="-1"></a>libraries <span class="ot">&lt;-</span> <span class="fu">c</span>(<span class="st">"ggplot2"</span>, <span class="st">"hrbrthemes"</span>, <span class="st">"dplyr"</span>, <span class="st">"tidyr"</span>, <span class="st">"viridis"</span>, </span>
<span id="cb1-4"><a href="#cb1-4" aria-hidden="true" tabindex="-1"></a>               <span class="st">"readr"</span>, <span class="st">"magick"</span>, <span class="st">"scales"</span>, <span class="st">"randomNames"</span>, <span class="st">"ggrepel"</span>, </span>
<span id="cb1-5"><a href="#cb1-5" aria-hidden="true" tabindex="-1"></a>               <span class="st">"stringr"</span>, <span class="st">"gridExtra"</span>, <span class="st">"gt"</span>)</span>
<span id="cb1-6"><a href="#cb1-6" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb1-7"><a href="#cb1-7" aria-hidden="true" tabindex="-1"></a><span class="co"># Install missing packages</span></span>
<span id="cb1-8"><a href="#cb1-8" aria-hidden="true" tabindex="-1"></a>missing_packages <span class="ot">&lt;-</span> libraries[<span class="sc">!</span>(libraries <span class="sc">%in%</span> <span class="fu">installed.packages</span>()[,<span class="st">"Package"</span>])]</span>
<span id="cb1-9"><a href="#cb1-9" aria-hidden="true" tabindex="-1"></a><span class="cf">if</span>(<span class="fu">length</span>(missing_packages)) <span class="fu">install.packages</span>(missing_packages, <span class="at">dependencies =</span> <span class="cn">TRUE</span>)</span>
<span id="cb1-10"><a href="#cb1-10" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb1-11"><a href="#cb1-11" aria-hidden="true" tabindex="-1"></a><span class="co"># Load required libraries</span></span>
<span id="cb1-12"><a href="#cb1-12" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(ggplot2)      <span class="co"># Data visualization</span></span>
<span id="cb1-13"><a href="#cb1-13" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(hrbrthemes)   <span class="co"># Themes for ggplot2</span></span>
<span id="cb1-14"><a href="#cb1-14" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(dplyr)        <span class="co"># Data manipulation</span></span>
<span id="cb1-15"><a href="#cb1-15" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(tidyr)        <span class="co"># Data tidying</span></span>
<span id="cb1-16"><a href="#cb1-16" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(viridis)      <span class="co"># Color palettes</span></span>
<span id="cb1-17"><a href="#cb1-17" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(readr)        <span class="co"># Data import</span></span>
<span id="cb1-18"><a href="#cb1-18" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(magick)       <span class="co"># Image processing</span></span>
<span id="cb1-19"><a href="#cb1-19" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(scales)       <span class="co"># Scaling functions</span></span>
<span id="cb1-20"><a href="#cb1-20" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(randomNames)  <span class="co"># Generate random names</span></span>
<span id="cb1-21"><a href="#cb1-21" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(ggrepel)      <span class="co"># Repel overlapping text labels</span></span>
<span id="cb1-22"><a href="#cb1-22" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(stringr)      <span class="co"># String manipulation</span></span>
<span id="cb1-23"><a href="#cb1-23" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(gridExtra)    <span class="co"># Arrange multiple plots</span></span>
<span id="cb1-24"><a href="#cb1-24" aria-hidden="true" tabindex="-1"></a><span class="fu">library</span>(gt)           <span class="co"># Create tables</span></span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
</div>
<p>import two csv containing bins quality statistics</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb2"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true" tabindex="-1"></a>path_50_10_bins_stats_92 <span class="ot">&lt;-</span> <span class="st">"/project/asteen_1130/deep_vs_surface/manual_results/07_bin_refinement/SRR7066492/metawrap_50_10_bins.stats"</span></span>
<span id="cb2-2"><a href="#cb2-2" aria-hidden="true" tabindex="-1"></a>data_50_10_bins_stats_92 <span class="ot">&lt;-</span> <span class="fu">read_tsv</span>(path_50_10_bins_stats_92) <span class="sc">|&gt;</span> <span class="fu">mutate</span>(<span class="at">location =</span> <span class="st">"deep"</span>)</span>
<span id="cb2-3"><a href="#cb2-3" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb2-4"><a href="#cb2-4" aria-hidden="true" tabindex="-1"></a>path_50_10_bins_stats_93 <span class="ot">&lt;-</span> <span class="st">"/project/asteen_1130/deep_vs_surface/manual_results/07_bin_refinement/SRR7066493/metawrap_50_10_bins.stats"</span></span>
<span id="cb2-5"><a href="#cb2-5" aria-hidden="true" tabindex="-1"></a>data_50_10_bins_stats_93 <span class="ot">&lt;-</span> <span class="fu">read_tsv</span>(path_50_10_bins_stats_93) <span class="sc">|&gt;</span> <span class="fu">mutate</span>(<span class="at">location =</span> <span class="st">"surface"</span>)</span>
<span id="cb2-6"><a href="#cb2-6" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb2-7"><a href="#cb2-7" aria-hidden="true" tabindex="-1"></a>combined_data <span class="ot">&lt;-</span> <span class="fu">bind_rows</span>(data_50_10_bins_stats_92, data_50_10_bins_stats_93)</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
</div>
</section>
<section id="Overview" class="level4">
<h4 class="anchored" data-anchor-id="Overview">Overview</h4>
<div class="callout callout-style-default callout-note callout-titled">
<div class="callout-header d-flex align-content-center">
<div class="callout-icon-container">
<i class="callout-icon"></i>
</div>
<div class="callout-title-container flex-fill">
Note
</div>
</div>
<div class="callout-body-container callout-body">
<p>We identified 29 metagenome-assembled genome (MAG) bins in the surface sample (SRR7066493) and 21 bins in the deep sample (SRR7066492).</p>
<p>Bins with at least 50% completeness and no more than 10% contamination are classified as medium-quality MAGs. In the visualization, completeness and contamination levels of each bin are represented, with the green region highlighting high-quality bins (completeness &gt; 90% and contamination &lt; 5%). All other bins fall into the medium-quality category.</p>
<p>The bin origins are also depicted: circles represent bins from the deep sample, while triangles correspond to bins from the surface sample. Additionally, bin lineage information is conveyed through color coding, allowing for taxonomic differentiation.</p>
<div class="callout callout-style-default callout-tip callout-titled" title="Reference">
<div class="callout-header d-flex align-content-center" data-bs-toggle="collapse" data-bs-target=".callout-2-contents" aria-controls="callout-2" aria-expanded="false" aria-label="Toggle callout">
<div class="callout-icon-container">
<i class="callout-icon"></i>
</div>
<div class="callout-title-container flex-fill">
Reference
</div>
<div class="callout-btn-toggle d-inline-block border-0 py-1 ps-1 pe-0 float-end"><i class="callout-toggle"></i></div>
</div>
<div id="callout-2" class="callout-2-contents callout-collapse collapse">
<div class="callout-body-container callout-body">
<p>The Genome Standards Consortium, Robert M. Bowers, Nikos C. Kyrpides, Ramunas Stepanauskas, Miranda Harmon-Smith, Devin Doud, T. B. K. Reddy, Frederik Schulz, Jessica Jarett, Adam R. Rivers, Emiley A. Eloe-Fadrosh, Susannah G. Tringe, Natalia N. Ivanova, Alex Copeland, Alicia Clum, Eric D. Becraft, Rex R. Malmstrom, Bruce Birren, Mircea Podar, Peer Bork, George M. Weinstock, George M. Garrity, Jeremy A. Dodsworth, Shibu Yooseph, Granger Sutton, Frank O. Glöckner, Jack A. Gilbert, William C. Nelson, Steven J. Hallam, Sean P. Jungbluth, Thijs J. G. Ettema, Scott Tighe, Konstantinos T. Konstantinidis, Wen-Tso Liu, Brett J. Baker, Thomas Rattei, Jonathan A. Eisen, Brian Hedlund, Katherine D. McMahon, Noah Fierer, Rob Knight, Rob Finn, Guy Cochrane, Ilene Karsch-Mizrachi, Gene W. Tyson, Christian Rinke, Alla Lapidus, Folker Meyer, Pelin Yilmaz, Donovan H. Parks, A. Murat Eren, Lynn Schriml, Jillian F. Banfield, Philip Hugenholtz, and Tanja Woyke. 2017. “Minimum Information about a Single Amplified Genome (MISAG) and a Metagenome-Assembled Genome (MIMAG) of Bacteria and Archaea.” Nature Biotechnology 35(8):725–31. doi: 10.1038/nbt.3893.</p>
</div>
</div>
</div>
<div class="cell">
<div class="sourceCode cell-code" id="cb3"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb3-1"><a href="#cb3-1" aria-hidden="true" tabindex="-1"></a>scatter_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(combined_data, <span class="fu">aes</span>(<span class="at">x =</span> completeness, <span class="at">y =</span> contamination)) <span class="sc">+</span></span>
<span id="cb3-2"><a href="#cb3-2" aria-hidden="true" tabindex="-1"></a>  <span class="fu">geom_point</span>(<span class="fu">aes</span>(<span class="at">color =</span> lineage, <span class="at">shape =</span> location),<span class="at">size =</span> <span class="fl">1.5</span>) <span class="sc">+</span> </span>
<span id="cb3-3"><a href="#cb3-3" aria-hidden="true" tabindex="-1"></a>  <span class="co"># Add axes labels, title, and subtitle</span></span>
<span id="cb3-4"><a href="#cb3-4" aria-hidden="true" tabindex="-1"></a>  <span class="fu">labs</span>(</span>
<span id="cb3-5"><a href="#cb3-5" aria-hidden="true" tabindex="-1"></a>    <span class="at">title =</span> <span class="st">"Bin Quality Data Visualization"</span>,</span>
<span id="cb3-6"><a href="#cb3-6" aria-hidden="true" tabindex="-1"></a>    <span class="at">subtitle =</span> <span class="st">"completeness and contamination"</span>,</span>
<span id="cb3-7"><a href="#cb3-7" aria-hidden="true" tabindex="-1"></a>    <span class="at">x =</span> <span class="st">"completeness (%)"</span>,</span>
<span id="cb3-8"><a href="#cb3-8" aria-hidden="true" tabindex="-1"></a>    <span class="at">y =</span> <span class="st">"contamination (%)"</span>) <span class="sc">+</span> </span>
<span id="cb3-9"><a href="#cb3-9" aria-hidden="true" tabindex="-1"></a>  <span class="fu">geom_rect</span>(<span class="fu">aes</span>(<span class="at">xmin =</span> <span class="dv">90</span>, <span class="at">xmax =</span> <span class="cn">Inf</span>, <span class="at">ymin =</span> <span class="dv">0</span>, <span class="at">ymax =</span> <span class="dv">5</span>), <span class="at">fill =</span> <span class="st">"light green"</span>, <span class="at">alpha =</span> <span class="fl">0.01</span>) <span class="sc">+</span></span>
<span id="cb3-10"><a href="#cb3-10" aria-hidden="true" tabindex="-1"></a>  <span class="fu">labs</span>(<span class="at">color =</span> <span class="st">"* Green region are High-quality bins "</span>) <span class="sc">+</span></span>
<span id="cb3-11"><a href="#cb3-11" aria-hidden="true" tabindex="-1"></a>  <span class="fu">theme</span>(<span class="at">legend.title =</span> <span class="fu">element_text</span>(<span class="at">size =</span> <span class="dv">9</span>))</span>
<span id="cb3-12"><a href="#cb3-12" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb3-13"><a href="#cb3-13" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb3-14"><a href="#cb3-14" aria-hidden="true" tabindex="-1"></a>combined_data<span class="sc">$</span>bin_quality <span class="ot">&lt;-</span> <span class="st">"Medium-quality bins"</span></span>
<span id="cb3-15"><a href="#cb3-15" aria-hidden="true" tabindex="-1"></a>combined_data<span class="sc">$</span>bin_quality[combined_data<span class="sc">$</span>completeness<span class="sc">&gt;</span><span class="dv">90</span> <span class="sc">&amp;</span> combined_data<span class="sc">$</span>contamination<span class="sc">&lt;</span><span class="dv">5</span>] <span class="ot">&lt;-</span> <span class="st">"High-quality bins"</span></span>
<span id="cb3-16"><a href="#cb3-16" aria-hidden="true" tabindex="-1"></a>freq_table <span class="ot">&lt;-</span> <span class="fu">table</span>(combined_data<span class="sc">$</span>location, combined_data<span class="sc">$</span>bin_quality)</span>
<span id="cb3-17"><a href="#cb3-17" aria-hidden="true" tabindex="-1"></a>table_grob <span class="ot">&lt;-</span> <span class="fu">tableGrob</span>(freq_table)</span>
<span id="cb3-18"><a href="#cb3-18" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb3-19"><a href="#cb3-19" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb3-20"><a href="#cb3-20" aria-hidden="true" tabindex="-1"></a><span class="co"># show both</span></span>
<span id="cb3-21"><a href="#cb3-21" aria-hidden="true" tabindex="-1"></a><span class="fu">grid.arrange</span>(scatter_plot, table_grob, <span class="at">nrow=</span><span class="dv">2</span>, <span class="at">heights=</span><span class="fu">c</span>(<span class="dv">5</span>, <span class="dv">1</span>))</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-3-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</div>
</div>
</section>
<section id="completeness" class="level3">
<h3 class="anchored" data-anchor-id="completeness">completeness</h3>
<p>Completeness in metagenome assembly refers to the extent to which the assembled contigs or scaffolds represent the total genomic content of the sampled microbial community. here we first try to compare distribution of completeness in both samples.</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb4"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true" tabindex="-1"></a>completeness_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data =</span> combined_data, <span class="fu">aes</span>(<span class="at">x =</span> location, <span class="at">y =</span> completeness)) <span class="sc">+</span></span>
<span id="cb4-2"><a href="#cb4-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_boxplot</span>(<span class="at">alpha =</span> <span class="fl">0.6</span>) <span class="sc">+</span>  <span class="co"># Adjust transparency if needed</span></span>
<span id="cb4-3"><a href="#cb4-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_ipsum</span>() <span class="sc">+</span></span>
<span id="cb4-4"><a href="#cb4-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_jitter</span>(<span class="at">width =</span> <span class="fl">0.2</span>, <span class="at">alpha =</span> <span class="fl">0.5</span>) <span class="sc">+</span></span>
<span id="cb4-5"><a href="#cb4-5" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">x =</span> <span class="st">""</span>, <span class="at">y =</span> <span class="st">"Completeness"</span>)  <span class="co"># Removing x-axis label for clarity</span></span>
<span id="cb4-6"><a href="#cb4-6" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb4-7"><a href="#cb4-7" aria-hidden="true" tabindex="-1"></a>completeness_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-4-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="bins_completeness_scores_based_on_lineage" class="level3">
<h3 class="anchored" data-anchor-id="bins_completeness_scores_based_on_lineage">bins completeness scores based on lineage</h3>
<p>Here, we present the distribution of completeness scores across different lineages, highlighting variations between surface and deep samples. Notably, the deep samples exhibit a substantial number of archaeal lineages with high completeness, indicating their strong representation in these environments. In contrast, within the surface samples, the Euryarchaeota lineage demonstrates particularly high completeness, suggesting its dominance or prevalence in these conditions.</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb5"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb5-1"><a href="#cb5-1" aria-hidden="true" tabindex="-1"></a>completeness_lineage_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data=</span>combined_data, <span class="fu">aes</span>(<span class="at">x=</span>lineage, <span class="at">y=</span>completeness, <span class="at">fill=</span>lineage)) <span class="sc">+</span></span>
<span id="cb5-2"><a href="#cb5-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_violin</span>(<span class="at">alpha=</span><span class="fl">0.5</span>, <span class="at">trim=</span><span class="cn">FALSE</span>) <span class="sc">+</span></span>
<span id="cb5-3"><a href="#cb5-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_jitter</span>(<span class="at">width=</span><span class="fl">0.1</span>, <span class="at">alpha=</span><span class="fl">0.5</span>, <span class="at">size=</span><span class="dv">2</span>) <span class="sc">+</span>  <span class="co"># Adds individual data points</span></span>
<span id="cb5-4"><a href="#cb5-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">facet_wrap</span>(<span class="sc">~</span>location) <span class="sc">+</span></span>
<span id="cb5-5"><a href="#cb5-5" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_minimal</span>() <span class="sc">+</span></span>
<span id="cb5-6"><a href="#cb5-6" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme</span>(</span>
<span id="cb5-7"><a href="#cb5-7" aria-hidden="true" tabindex="-1"></a>        <span class="at">legend.position =</span> <span class="st">"none"</span>,</span>
<span id="cb5-8"><a href="#cb5-8" aria-hidden="true" tabindex="-1"></a>        <span class="at">axis.text.x =</span> <span class="fu">element_text</span>(<span class="at">angle =</span> <span class="dv">45</span>, <span class="at">hjust =</span> <span class="dv">1</span>)  <span class="co"># Rotates x-axis text</span></span>
<span id="cb5-9"><a href="#cb5-9" aria-hidden="true" tabindex="-1"></a>    ) <span class="sc">+</span></span>
<span id="cb5-10"><a href="#cb5-10" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">y =</span> <span class="st">"completeness"</span>, <span class="at">x =</span> <span class="st">"Lineage"</span>, <span class="at">title =</span> <span class="st">"completeness by Lineage"</span>)</span>
<span id="cb5-11"><a href="#cb5-11" aria-hidden="true" tabindex="-1"></a>completeness_lineage_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-5-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="contamination" class="level3">
<h3 class="anchored" data-anchor-id="contamination">contamination</h3>
<p>the unwanted sequences in the bins that do not originate from the target microbial community.Let’s have a overall look at contamination in two samples.</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb6"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true" tabindex="-1"></a>contamination_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data =</span> combined_data, <span class="fu">aes</span>(<span class="at">x =</span> location, <span class="at">y =</span> contamination)) <span class="sc">+</span></span>
<span id="cb6-2"><a href="#cb6-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_boxplot</span>(<span class="at">alpha =</span> <span class="fl">0.6</span>) <span class="sc">+</span>  <span class="co"># Adjust transparency if needed</span></span>
<span id="cb6-3"><a href="#cb6-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_ipsum</span>() <span class="sc">+</span></span>
<span id="cb6-4"><a href="#cb6-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_jitter</span>(<span class="at">width =</span> <span class="fl">0.2</span>, <span class="at">alpha =</span> <span class="fl">0.5</span>) <span class="sc">+</span></span>
<span id="cb6-5"><a href="#cb6-5" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">x =</span> <span class="st">""</span>, <span class="at">y =</span> <span class="st">"contamination"</span>)  <span class="co"># Removing x-axis label for clarity</span></span>
<span id="cb6-6"><a href="#cb6-6" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb6-7"><a href="#cb6-7" aria-hidden="true" tabindex="-1"></a>contamination_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-6-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="bins_contamination_scores_based_on_lineage" class="level3">
<h3 class="anchored" data-anchor-id="bins_contamination_scores_based_on_lineage">bins contamination scores based on lineage</h3>
<p>Here, we present the distribution of contamination scores across different lineages The first thing that caught my attention was the level of contamination in Deltaproteobacteria. My initial thought is that the sequence may have high genomic diversity and complexity, or it might share many genomic features with other taxa. Alternatively, it could be due to low abundance and assembly artifacts.</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb7"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb7-1"><a href="#cb7-1" aria-hidden="true" tabindex="-1"></a>contamination_lineage_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data=</span>combined_data, <span class="fu">aes</span>(<span class="at">x=</span>lineage, <span class="at">y=</span>contamination, <span class="at">fill=</span>lineage)) <span class="sc">+</span></span>
<span id="cb7-2"><a href="#cb7-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_violin</span>(<span class="at">alpha=</span><span class="fl">0.5</span>, <span class="at">trim=</span><span class="cn">FALSE</span>) <span class="sc">+</span></span>
<span id="cb7-3"><a href="#cb7-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_jitter</span>(<span class="at">width=</span><span class="fl">0.1</span>, <span class="at">alpha=</span><span class="fl">0.5</span>, <span class="at">size=</span><span class="dv">2</span>) <span class="sc">+</span>  <span class="co"># Adds individual data points</span></span>
<span id="cb7-4"><a href="#cb7-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">facet_wrap</span>(<span class="sc">~</span>location) <span class="sc">+</span></span>
<span id="cb7-5"><a href="#cb7-5" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_minimal</span>() <span class="sc">+</span></span>
<span id="cb7-6"><a href="#cb7-6" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme</span>(</span>
<span id="cb7-7"><a href="#cb7-7" aria-hidden="true" tabindex="-1"></a>        <span class="at">legend.position =</span> <span class="st">"none"</span>,</span>
<span id="cb7-8"><a href="#cb7-8" aria-hidden="true" tabindex="-1"></a>        <span class="at">axis.text.x =</span> <span class="fu">element_text</span>(<span class="at">angle =</span> <span class="dv">45</span>, <span class="at">hjust =</span> <span class="dv">1</span>)  <span class="co"># Rotates x-axis text</span></span>
<span id="cb7-9"><a href="#cb7-9" aria-hidden="true" tabindex="-1"></a>    ) <span class="sc">+</span></span>
<span id="cb7-10"><a href="#cb7-10" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">y =</span> <span class="st">"contamination"</span>, <span class="at">x =</span> <span class="st">"Lineage"</span>, <span class="at">title =</span> <span class="st">"contamination by Lineage"</span>)</span>
<span id="cb7-11"><a href="#cb7-11" aria-hidden="true" tabindex="-1"></a>contamination_lineage_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-7-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="bins_CG_content_based_on_lineage" class="level3">
<h3 class="anchored" data-anchor-id="bins_CG_content_based_on_lineage">bins CG content based on lineage</h3>
<div class="cell">
<div class="sourceCode cell-code" id="cb8"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true" tabindex="-1"></a>CG_content_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data =</span> combined_data, <span class="fu">aes</span>(<span class="at">x =</span> location, <span class="at">y =</span> GC)) <span class="sc">+</span></span>
<span id="cb8-2"><a href="#cb8-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_boxplot</span>(<span class="at">alpha =</span> <span class="fl">0.6</span>) <span class="sc">+</span>  <span class="co"># Adjust transparency if needed</span></span>
<span id="cb8-3"><a href="#cb8-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_ipsum</span>() <span class="sc">+</span></span>
<span id="cb8-4"><a href="#cb8-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_jitter</span>(<span class="at">width =</span> <span class="fl">0.2</span>, <span class="at">alpha =</span> <span class="fl">0.5</span>) <span class="sc">+</span></span>
<span id="cb8-5"><a href="#cb8-5" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">x =</span> <span class="st">""</span>, <span class="at">y =</span> <span class="st">"CG content"</span>)  <span class="co"># Removing x-axis label for clarity</span></span>
<span id="cb8-6"><a href="#cb8-6" aria-hidden="true" tabindex="-1"></a></span>
<span id="cb8-7"><a href="#cb8-7" aria-hidden="true" tabindex="-1"></a>CG_content_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-8-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
<div class="cell">
<div class="sourceCode cell-code" id="cb9"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb9-1"><a href="#cb9-1" aria-hidden="true" tabindex="-1"></a>contamination_lineage_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data=</span>combined_data, <span class="fu">aes</span>(<span class="at">x=</span>lineage, <span class="at">y=</span>contamination, <span class="at">fill=</span>lineage)) <span class="sc">+</span></span>
<span id="cb9-2"><a href="#cb9-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_violin</span>(<span class="at">alpha=</span><span class="fl">0.5</span>, <span class="at">trim=</span><span class="cn">FALSE</span>) <span class="sc">+</span></span>
<span id="cb9-3"><a href="#cb9-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_jitter</span>(<span class="at">width=</span><span class="fl">0.1</span>, <span class="at">alpha=</span><span class="fl">0.5</span>, <span class="at">size=</span><span class="dv">2</span>) <span class="sc">+</span>  <span class="co"># Adds individual data points</span></span>
<span id="cb9-4"><a href="#cb9-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">facet_wrap</span>(<span class="sc">~</span>location) <span class="sc">+</span></span>
<span id="cb9-5"><a href="#cb9-5" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_minimal</span>() <span class="sc">+</span></span>
<span id="cb9-6"><a href="#cb9-6" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme</span>(</span>
<span id="cb9-7"><a href="#cb9-7" aria-hidden="true" tabindex="-1"></a>        <span class="at">legend.position =</span> <span class="st">"none"</span>,</span>
<span id="cb9-8"><a href="#cb9-8" aria-hidden="true" tabindex="-1"></a>        <span class="at">axis.text.x =</span> <span class="fu">element_text</span>(<span class="at">angle =</span> <span class="dv">45</span>, <span class="at">hjust =</span> <span class="dv">1</span>)  <span class="co"># Rotates x-axis text</span></span>
<span id="cb9-9"><a href="#cb9-9" aria-hidden="true" tabindex="-1"></a>    ) <span class="sc">+</span></span>
<span id="cb9-10"><a href="#cb9-10" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">y =</span> <span class="st">"contamination"</span>, <span class="at">x =</span> <span class="st">"Lineage"</span>, <span class="at">title =</span> <span class="st">"contamination by Lineage"</span>)</span>
<span id="cb9-11"><a href="#cb9-11" aria-hidden="true" tabindex="-1"></a>contamination_lineage_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-9-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="Bin_size" class="level3">
<h3 class="anchored" data-anchor-id="Bin_size">Bin size</h3>
<p>numbers are in million. Here we see the size of bins in million base and the frequency of each, we have some bigger bins in deep sample but the frequency of overall big bins is higher in surface sample.</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb10"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true" tabindex="-1"></a>size_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data=</span>combined_data, <span class="fu">aes</span>(<span class="at">x=</span>size)) <span class="sc">+</span></span>
<span id="cb10-2"><a href="#cb10-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_histogram</span>( <span class="at">alpha=</span><span class="fl">0.6</span>, <span class="at">position =</span> <span class="st">'identity'</span>) <span class="sc">+</span></span>
<span id="cb10-3"><a href="#cb10-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">facet_wrap</span>(<span class="sc">~</span>location) <span class="sc">+</span></span>
<span id="cb10-4"><a href="#cb10-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">scale_x_continuous</span>(</span>
<span id="cb10-5"><a href="#cb10-5" aria-hidden="true" tabindex="-1"></a>        <span class="at">labels =</span> scales<span class="sc">::</span><span class="fu">label_number</span>(<span class="at">scale_cut =</span> scales<span class="sc">::</span><span class="fu">cut_si</span>(<span class="st">""</span>))</span>
<span id="cb10-6"><a href="#cb10-6" aria-hidden="true" tabindex="-1"></a>    ) <span class="sc">+</span></span>
<span id="cb10-7"><a href="#cb10-7" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_ipsum</span>() <span class="sc">+</span></span>
<span id="cb10-8"><a href="#cb10-8" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">x =</span> <span class="st">"Size"</span>, <span class="at">y =</span> <span class="st">"Count"</span>, <span class="at">fill =</span> <span class="st">""</span>)</span>
<span id="cb10-9"><a href="#cb10-9" aria-hidden="true" tabindex="-1"></a>size_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-10-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="N50" class="level3">
<h3 class="anchored" data-anchor-id="N50">N50</h3>
<p>N50 describes the quality of assembled genomes or contigs. It refers to the length at which 50% of the assembled bases are contained in sequences at or above that length. describe the quality of assembled genomes or contigs. It refers to the length at which 50% of the assembled bases are contained in sequences at or above that length.</p>
<div class="cell">
<div class="sourceCode cell-code" id="cb11"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb11-1"><a href="#cb11-1" aria-hidden="true" tabindex="-1"></a>N50_plot <span class="ot">&lt;-</span> <span class="fu">ggplot</span>(<span class="at">data=</span>combined_data, <span class="fu">aes</span>(<span class="at">x=</span>N50)) <span class="sc">+</span></span>
<span id="cb11-2"><a href="#cb11-2" aria-hidden="true" tabindex="-1"></a>    <span class="fu">geom_histogram</span>( <span class="at">alpha=</span><span class="fl">0.6</span>, <span class="at">position =</span> <span class="st">'identity'</span>) <span class="sc">+</span></span>
<span id="cb11-3"><a href="#cb11-3" aria-hidden="true" tabindex="-1"></a>    <span class="fu">facet_wrap</span>(<span class="sc">~</span>location) <span class="sc">+</span></span>
<span id="cb11-4"><a href="#cb11-4" aria-hidden="true" tabindex="-1"></a>    <span class="fu">scale_x_continuous</span>(</span>
<span id="cb11-5"><a href="#cb11-5" aria-hidden="true" tabindex="-1"></a>        <span class="at">labels =</span> scales<span class="sc">::</span><span class="fu">label_number</span>(<span class="at">scale_cut =</span> scales<span class="sc">::</span><span class="fu">cut_si</span>(<span class="st">""</span>))</span>
<span id="cb11-6"><a href="#cb11-6" aria-hidden="true" tabindex="-1"></a>    ) <span class="sc">+</span></span>
<span id="cb11-7"><a href="#cb11-7" aria-hidden="true" tabindex="-1"></a>    <span class="fu">theme_ipsum</span>() <span class="sc">+</span></span>
<span id="cb11-8"><a href="#cb11-8" aria-hidden="true" tabindex="-1"></a>    <span class="fu">labs</span>(<span class="at">x =</span> <span class="st">"N50"</span>, <span class="at">y =</span> <span class="st">"Count"</span>, <span class="at">fill =</span> <span class="st">""</span>)</span>
<span id="cb11-9"><a href="#cb11-9" aria-hidden="true" tabindex="-1"></a>N50_plot</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-11-1.png" class="img-fluid figure-img" width="672"></p>
</figure>
</div>
</div>
</div>
</section>
<section id="Bin_Comparision" class="level3">
<h3 class="anchored" data-anchor-id="Bin_Comparision">Bin Comparision</h3>
<div class="cell">
<div class="sourceCode cell-code" id="cb12"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true" tabindex="-1"></a>binning_results_compare <span class="ot">&lt;-</span> <span class="fu">image_read</span>(<span class="st">"/project/asteen_1130/deep_vs_surface/manual_results/07_bin_refinement/SRR7066492/figures/binning_results.png"</span>)</span>
<span id="cb12-2"><a href="#cb12-2" aria-hidden="true" tabindex="-1"></a><span class="fu">print</span>(binning_results_compare)</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output cell-output-stdout">
<pre><code># A tibble: 1 × 7
  format width height colorspace matte filesize density  
  &lt;chr&gt;  &lt;int&gt;  &lt;int&gt; &lt;chr&gt;      &lt;lgl&gt;    &lt;int&gt; &lt;chr&gt;    
1 PNG     4800   2400 sRGB       TRUE    501959 +118x+118</code></pre>
</div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-12-1.png" class="img-fluid figure-img"></p>
</figure>
</div>
</div>
<div class="sourceCode cell-code" id="cb14"><pre class="sourceCode r code-with-copy"><code class="sourceCode r"><span id="cb14-1"><a href="#cb14-1" aria-hidden="true" tabindex="-1"></a>intermediate_binning_results_compare <span class="ot">&lt;-</span> <span class="fu">image_read</span>(<span class="st">"/project/asteen_1130/deep_vs_surface/manual_results/07_bin_refinement/SRR7066492/figures/intermediate_binning_results.png"</span>)</span>
<span id="cb14-2"><a href="#cb14-2" aria-hidden="true" tabindex="-1"></a><span class="fu">print</span>(intermediate_binning_results_compare)</span></code><button title="Copy to Clipboard" class="code-copy-button"><i class="bi"></i></button></pre></div>
<div class="cell-output cell-output-stdout">
<pre><code># A tibble: 1 × 7
  format width height colorspace matte filesize density  
  &lt;chr&gt;  &lt;int&gt;  &lt;int&gt; &lt;chr&gt;      &lt;lgl&gt;    &lt;int&gt; &lt;chr&gt;    
1 PNG     4800   2400 sRGB       TRUE    541789 +118x+118</code></pre>
</div>
<div class="cell-output-display">
<div>
<figure class="figure">
<p><img src="main_files/figure-html/unnamed-chunk-12-2.png" class="img-fluid figure-img"></p>
</figure>
</div>
</div>
</div>


