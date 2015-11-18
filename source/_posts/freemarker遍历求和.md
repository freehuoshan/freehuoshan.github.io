title: freemarker遍历求和
date: 2015-11-10 21:16:38
tags: freemarker
---
> freemarker使用list遍历求和:



        <#assign pageall=0 page=0 page1=0>

        <#list geneReportList as report>

                <#assign pageall= pageall+report.genelist.size()+1>
                <#assign pagezun=pageall+2>
                <#assign page = pageall - report.genelist.size()>
                <#if geneReport_index == report_index>
                        <#assign page1=page>
                </#if>

        </#list>

        P${pageall+2}-${page1}

