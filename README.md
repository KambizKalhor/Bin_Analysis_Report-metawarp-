<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en"><head>

<meta charset="utf-8">
<meta name="generator" content="quarto-1.5.57">

<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">


<title>Bin Quality Report</title>
<style>
code{white-space: pre-wrap;}
span.smallcaps{font-variant: small-caps;}
div.columns{display: flex; gap: min(4vw, 1.5em);}
div.column{flex: auto; overflow-x: auto;}
div.hanging-indent{margin-left: 1.5em; text-indent: -1.5em;}
ul.task-list{list-style: none;}
ul.task-list li input[type="checkbox"] {
  width: 0.8em;
  margin: 0 0.8em 0.2em -1em; /* quarto-specific, see https://github.com/quarto-dev/quarto-cli/issues/4556 */ 
  vertical-align: middle;
}
/* CSS for syntax highlighting */
pre > code.sourceCode { white-space: pre; position: relative; }
pre > code.sourceCode > span { line-height: 1.25; }
pre > code.sourceCode > span:empty { height: 1.2em; }
.sourceCode { overflow: visible; }
code.sourceCode > span { color: inherit; text-decoration: inherit; }
div.sourceCode { margin: 1em 0; }
pre.sourceCode { margin: 0; }
@media screen {
div.sourceCode { overflow: auto; }
}
@media print {
pre > code.sourceCode { white-space: pre-wrap; }
pre > code.sourceCode > span { display: inline-block; text-indent: -5em; padding-left: 5em; }
}
pre.numberSource code
  { counter-reset: source-line 0; }
pre.numberSource code > span
  { position: relative; left: -4em; counter-increment: source-line; }
pre.numberSource code > span > a:first-child::before
  { content: counter(source-line);
    position: relative; left: -1em; text-align: right; vertical-align: baseline;
    border: none; display: inline-block;
    -webkit-touch-callout: none; -webkit-user-select: none;
    -khtml-user-select: none; -moz-user-select: none;
    -ms-user-select: none; user-select: none;
    padding: 0 4px; width: 4em;
  }
pre.numberSource { margin-left: 3em;  padding-left: 4px; }
div.sourceCode
  {   }
@media screen {
pre > code.sourceCode > span > a:first-child::before { text-decoration: underline; }
}
</style>


<script src="main_files/libs/clipboard/clipboard.min.js"></script>
<script src="main_files/libs/quarto-html/quarto.js"></script>
<script src="main_files/libs/quarto-html/popper.min.js"></script>
<script src="main_files/libs/quarto-html/tippy.umd.min.js"></script>
<script src="main_files/libs/quarto-html/anchor.min.js"></script>
<link href="main_files/libs/quarto-html/tippy.css" rel="stylesheet">
<link href="main_files/libs/quarto-html/quarto-syntax-highlighting.css" rel="stylesheet" id="quarto-text-highlighting-styles">
<script src="main_files/libs/bootstrap/bootstrap.min.js"></script>
<link href="main_files/libs/bootstrap/bootstrap-icons.css" rel="stylesheet">
<link href="main_files/libs/bootstrap/bootstrap.min.css" rel="stylesheet" id="quarto-bootstrap" data-mode="light">


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
</section>

</main>
<!-- /main column -->
<script id="quarto-html-after-body" type="application/javascript">
window.document.addEventListener("DOMContentLoaded", function (event) {
  const toggleBodyColorMode = (bsSheetEl) => {
    const mode = bsSheetEl.getAttribute("data-mode");
    const bodyEl = window.document.querySelector("body");
    if (mode === "dark") {
      bodyEl.classList.add("quarto-dark");
      bodyEl.classList.remove("quarto-light");
    } else {
      bodyEl.classList.add("quarto-light");
      bodyEl.classList.remove("quarto-dark");
    }
  }
  const toggleBodyColorPrimary = () => {
    const bsSheetEl = window.document.querySelector("link#quarto-bootstrap");
    if (bsSheetEl) {
      toggleBodyColorMode(bsSheetEl);
    }
  }
  toggleBodyColorPrimary();  
  const icon = "";
  const anchorJS = new window.AnchorJS();
  anchorJS.options = {
    placement: 'right',
    icon: icon
  };
  anchorJS.add('.anchored');
  const isCodeAnnotation = (el) => {
    for (const clz of el.classList) {
      if (clz.startsWith('code-annotation-')) {                     
        return true;
      }
    }
    return false;
  }
  const onCopySuccess = function(e) {
    // button target
    const button = e.trigger;
    // don't keep focus
    button.blur();
    // flash "checked"
    button.classList.add('code-copy-button-checked');
    var currentTitle = button.getAttribute("title");
    button.setAttribute("title", "Copied!");
    let tooltip;
    if (window.bootstrap) {
      button.setAttribute("data-bs-toggle", "tooltip");
      button.setAttribute("data-bs-placement", "left");
      button.setAttribute("data-bs-title", "Copied!");
      tooltip = new bootstrap.Tooltip(button, 
        { trigger: "manual", 
          customClass: "code-copy-button-tooltip",
          offset: [0, -8]});
      tooltip.show();    
    }
    setTimeout(function() {
      if (tooltip) {
        tooltip.hide();
        button.removeAttribute("data-bs-title");
        button.removeAttribute("data-bs-toggle");
        button.removeAttribute("data-bs-placement");
      }
      button.setAttribute("title", currentTitle);
      button.classList.remove('code-copy-button-checked');
    }, 1000);
    // clear code selection
    e.clearSelection();
  }
  const getTextToCopy = function(trigger) {
      const codeEl = trigger.previousElementSibling.cloneNode(true);
      for (const childEl of codeEl.children) {
        if (isCodeAnnotation(childEl)) {
          childEl.remove();
        }
      }
      return codeEl.innerText;
  }
  const clipboard = new window.ClipboardJS('.code-copy-button:not([data-in-quarto-modal])', {
    text: getTextToCopy
  });
  clipboard.on('success', onCopySuccess);
  if (window.document.getElementById('quarto-embedded-source-code-modal')) {
    // For code content inside modals, clipBoardJS needs to be initialized with a container option
    // TODO: Check when it could be a function (https://github.com/zenorocha/clipboard.js/issues/860)
    const clipboardModal = new window.ClipboardJS('.code-copy-button[data-in-quarto-modal]', {
      text: getTextToCopy,
      container: window.document.getElementById('quarto-embedded-source-code-modal')
    });
    clipboardModal.on('success', onCopySuccess);
  }
    var localhostRegex = new RegExp(/^(?:http|https):\/\/localhost\:?[0-9]*\//);
    var mailtoRegex = new RegExp(/^mailto:/);
      var filterRegex = new RegExp('/' + window.location.host + '/');
    var isInternal = (href) => {
        return filterRegex.test(href) || localhostRegex.test(href) || mailtoRegex.test(href);
    }
    // Inspect non-navigation links and adorn them if external
 	var links = window.document.querySelectorAll('a[href]:not(.nav-link):not(.navbar-brand):not(.toc-action):not(.sidebar-link):not(.sidebar-item-toggle):not(.pagination-link):not(.no-external):not([aria-hidden]):not(.dropdown-item):not(.quarto-navigation-tool):not(.about-link)');
    for (var i=0; i<links.length; i++) {
      const link = links[i];
      if (!isInternal(link.href)) {
        // undo the damage that might have been done by quarto-nav.js in the case of
        // links that we want to consider external
        if (link.dataset.originalHref !== undefined) {
          link.href = link.dataset.originalHref;
        }
      }
    }
  function tippyHover(el, contentFn, onTriggerFn, onUntriggerFn) {
    const config = {
      allowHTML: true,
      maxWidth: 500,
      delay: 100,
      arrow: false,
      appendTo: function(el) {
          return el.parentElement;
      },
      interactive: true,
      interactiveBorder: 10,
      theme: 'quarto',
      placement: 'bottom-start',
    };
    if (contentFn) {
      config.content = contentFn;
    }
    if (onTriggerFn) {
      config.onTrigger = onTriggerFn;
    }
    if (onUntriggerFn) {
      config.onUntrigger = onUntriggerFn;
    }
    window.tippy(el, config); 
  }
  const noterefs = window.document.querySelectorAll('a[role="doc-noteref"]');
  for (var i=0; i<noterefs.length; i++) {
    const ref = noterefs[i];
    tippyHover(ref, function() {
      // use id or data attribute instead here
      let href = ref.getAttribute('data-footnote-href') || ref.getAttribute('href');
      try { href = new URL(href).hash; } catch {}
      const id = href.replace(/^#\/?/, "");
      const note = window.document.getElementById(id);
      if (note) {
        return note.innerHTML;
      } else {
        return "";
      }
    });
  }
  const xrefs = window.document.querySelectorAll('a.quarto-xref');
  const processXRef = (id, note) => {
    // Strip column container classes
    const stripColumnClz = (el) => {
      el.classList.remove("page-full", "page-columns");
      if (el.children) {
        for (const child of el.children) {
          stripColumnClz(child);
        }
      }
    }
    stripColumnClz(note)
    if (id === null || id.startsWith('sec-')) {
      // Special case sections, only their first couple elements
      const container = document.createElement("div");
      if (note.children && note.children.length > 2) {
        container.appendChild(note.children[0].cloneNode(true));
        for (let i = 1; i < note.children.length; i++) {
          const child = note.children[i];
          if (child.tagName === "P" && child.innerText === "") {
            continue;
          } else {
            container.appendChild(child.cloneNode(true));
            break;
          }
        }
        if (window.Quarto?.typesetMath) {
          window.Quarto.typesetMath(container);
        }
        return container.innerHTML
      } else {
        if (window.Quarto?.typesetMath) {
          window.Quarto.typesetMath(note);
        }
        return note.innerHTML;
      }
    } else {
      // Remove any anchor links if they are present
      const anchorLink = note.querySelector('a.anchorjs-link');
      if (anchorLink) {
        anchorLink.remove();
      }
      if (window.Quarto?.typesetMath) {
        window.Quarto.typesetMath(note);
      }
      // TODO in 1.5, we should make sure this works without a callout special case
      if (note.classList.contains("callout")) {
        return note.outerHTML;
      } else {
        return note.innerHTML;
      }
    }
  }
  for (var i=0; i<xrefs.length; i++) {
    const xref = xrefs[i];
    tippyHover(xref, undefined, function(instance) {
      instance.disable();
      let url = xref.getAttribute('href');
      let hash = undefined; 
      if (url.startsWith('#')) {
        hash = url;
      } else {
        try { hash = new URL(url).hash; } catch {}
      }
      if (hash) {
        const id = hash.replace(/^#\/?/, "");
        const note = window.document.getElementById(id);
        if (note !== null) {
          try {
            const html = processXRef(id, note.cloneNode(true));
            instance.setContent(html);
          } finally {
            instance.enable();
            instance.show();
          }
        } else {
          // See if we can fetch this
          fetch(url.split('#')[0])
          .then(res => res.text())
          .then(html => {
            const parser = new DOMParser();
            const htmlDoc = parser.parseFromString(html, "text/html");
            const note = htmlDoc.getElementById(id);
            if (note !== null) {
              const html = processXRef(id, note);
              instance.setContent(html);
            } 
          }).finally(() => {
            instance.enable();
            instance.show();
          });
        }
      } else {
        // See if we can fetch a full url (with no hash to target)
        // This is a special case and we should probably do some content thinning / targeting
        fetch(url)
        .then(res => res.text())
        .then(html => {
          const parser = new DOMParser();
          const htmlDoc = parser.parseFromString(html, "text/html");
          const note = htmlDoc.querySelector('main.content');
          if (note !== null) {
            // This should only happen for chapter cross references
            // (since there is no id in the URL)
            // remove the first header
            if (note.children.length > 0 && note.children[0].tagName === "HEADER") {
              note.children[0].remove();
            }
            const html = processXRef(null, note);
            instance.setContent(html);
          } 
        }).finally(() => {
          instance.enable();
          instance.show();
        });
      }
    }, function(instance) {
    });
  }
      let selectedAnnoteEl;
      const selectorForAnnotation = ( cell, annotation) => {
        let cellAttr = 'data-code-cell="' + cell + '"';
        let lineAttr = 'data-code-annotation="' +  annotation + '"';
        const selector = 'span[' + cellAttr + '][' + lineAttr + ']';
        return selector;
      }
      const selectCodeLines = (annoteEl) => {
        const doc = window.document;
        const targetCell = annoteEl.getAttribute("data-target-cell");
        const targetAnnotation = annoteEl.getAttribute("data-target-annotation");
        const annoteSpan = window.document.querySelector(selectorForAnnotation(targetCell, targetAnnotation));
        const lines = annoteSpan.getAttribute("data-code-lines").split(",");
        const lineIds = lines.map((line) => {
          return targetCell + "-" + line;
        })
        let top = null;
        let height = null;
        let parent = null;
        if (lineIds.length > 0) {
            //compute the position of the single el (top and bottom and make a div)
            const el = window.document.getElementById(lineIds[0]);
            top = el.offsetTop;
            height = el.offsetHeight;
            parent = el.parentElement.parentElement;
          if (lineIds.length > 1) {
            const lastEl = window.document.getElementById(lineIds[lineIds.length - 1]);
            const bottom = lastEl.offsetTop + lastEl.offsetHeight;
            height = bottom - top;
          }
          if (top !== null && height !== null && parent !== null) {
            // cook up a div (if necessary) and position it 
            let div = window.document.getElementById("code-annotation-line-highlight");
            if (div === null) {
              div = window.document.createElement("div");
              div.setAttribute("id", "code-annotation-line-highlight");
              div.style.position = 'absolute';
              parent.appendChild(div);
            }
            div.style.top = top - 2 + "px";
            div.style.height = height + 4 + "px";
            div.style.left = 0;
            let gutterDiv = window.document.getElementById("code-annotation-line-highlight-gutter");
            if (gutterDiv === null) {
              gutterDiv = window.document.createElement("div");
              gutterDiv.setAttribute("id", "code-annotation-line-highlight-gutter");
              gutterDiv.style.position = 'absolute';
              const codeCell = window.document.getElementById(targetCell);
              const gutter = codeCell.querySelector('.code-annotation-gutter');
              gutter.appendChild(gutterDiv);
            }
            gutterDiv.style.top = top - 2 + "px";
            gutterDiv.style.height = height + 4 + "px";
          }
          selectedAnnoteEl = annoteEl;
        }
      };
      const unselectCodeLines = () => {
        const elementsIds = ["code-annotation-line-highlight", "code-annotation-line-highlight-gutter"];
        elementsIds.forEach((elId) => {
          const div = window.document.getElementById(elId);
          if (div) {
            div.remove();
          }
        });
        selectedAnnoteEl = undefined;
      };
        // Handle positioning of the toggle
    window.addEventListener(
      "resize",
      throttle(() => {
        elRect = undefined;
        if (selectedAnnoteEl) {
          selectCodeLines(selectedAnnoteEl);
        }
      }, 10)
    );
    function throttle(fn, ms) {
    let throttle = false;
    let timer;
      return (...args) => {
        if(!throttle) { // first call gets through
            fn.apply(this, args);
            throttle = true;
        } else { // all the others get throttled
            if(timer) clearTimeout(timer); // cancel #2
            timer = setTimeout(() => {
              fn.apply(this, args);
              timer = throttle = false;
            }, ms);
        }
      };
    }
      // Attach click handler to the DT
      const annoteDls = window.document.querySelectorAll('dt[data-target-cell]');
      for (const annoteDlNode of annoteDls) {
        annoteDlNode.addEventListener('click', (event) => {
          const clickedEl = event.target;
          if (clickedEl !== selectedAnnoteEl) {
            unselectCodeLines();
            const activeEl = window.document.querySelector('dt[data-target-cell].code-annotation-active');
            if (activeEl) {
              activeEl.classList.remove('code-annotation-active');
            }
            selectCodeLines(clickedEl);
            clickedEl.classList.add('code-annotation-active');
          } else {
            // Unselect the line
            unselectCodeLines();
            clickedEl.classList.remove('code-annotation-active');
          }
        });
      }
  const findCites = (el) => {
    const parentEl = el.parentElement;
    if (parentEl) {
      const cites = parentEl.dataset.cites;
      if (cites) {
        return {
          el,
          cites: cites.split(' ')
        };
      } else {
        return findCites(el.parentElement)
      }
    } else {
      return undefined;
    }
  };
  var bibliorefs = window.document.querySelectorAll('a[role="doc-biblioref"]');
  for (var i=0; i<bibliorefs.length; i++) {
    const ref = bibliorefs[i];
    const citeInfo = findCites(ref);
    if (citeInfo) {
      tippyHover(citeInfo.el, function() {
        var popup = window.document.createElement('div');
        citeInfo.cites.forEach(function(cite) {
          var citeDiv = window.document.createElement('div');
          citeDiv.classList.add('hanging-indent');
          citeDiv.classList.add('csl-entry');
          var biblioDiv = window.document.getElementById('ref-' + cite);
          if (biblioDiv) {
            citeDiv.innerHTML = biblioDiv.innerHTML;
          }
          popup.appendChild(citeDiv);
        });
        return popup.innerHTML;
      });
    }
  }
});
</script>
</div> <!-- /content -->




</body></html>
