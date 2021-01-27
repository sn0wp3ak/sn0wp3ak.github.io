---
layout: default
title: Word
---
<table align="center" border="0" width="70%">
         <tbody>
                 <!-- 标题 -->
                 <tr>
                         <td >
                                 <p align="center">
                                         <font size="7" color="#000000">
                                                 <b>Word</b>
                                         </font>
                                 </p>
                                 <hr>
                         </td>
                 </tr>
                 <!-- 简介 -->
                 <tr>
                         <td width="80%" align="center">
                                 <p align="center">
                                         <h2>日积跬步</h2>
                                         <h2>聚沙成塔</h2>
                                 </p>
                         </td>
                 </tr>
		 <!-- 词汇列表 -->
		 <tr>
		 	<td>
				<table width="30%" border="1px" align="center" cellspacing="5" cellpadding="5">
					<tbody>
							<div>
							{% for category in site.categories %}
								{% if category[0] == "Word" %}
									<ul>
										{% for post in category[1] %}
										<tr>
											<td width="100%" align="center">
												<a href="{{ post.url | prepend: site.baseurl | replace: '//', '/'}}">
													<b>{{ post.title }}</b>
												</a>
											</td>
										</tr>
										{% endfor %}
									</ul>
								{% endif %}
							{% endfor %}
							</div>
					</tbody>
				</table>
			</td>
		</tr> 
		<!-- 站内跳转 -->
                <tr>
                        <td>
                                <center>
                                        <p>&nbsp;</p>
                                        <table width="80%" align="center" cellpadding="5" cellspacing="5">
                                                <tbody>
                                                        <tr>
                                                                <td width="100%" align="center">
                                                                        <a href="/">
                                                                                <img src="/assets/home.png">
                                                                                <br>
                                                                                Go Home
                                                                        </a>
                                                                </td>
                                                        </tr>
                                                </tbody>
                                        </table>
                                        <hr>
                                        <font size="3" color="#000000">powered by jekyll</font>
                                </center>
                        </td>
                </tr>
	</tbody>
</table>
