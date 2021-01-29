---
layout: default
title: Tag
---
<table align="center" border="0" width="70%">
	<tbody>
		<!-- 标题 -->
		<tr>
			<td >
				<p align="center">
					<font size="7" color="#000000">
						<b>Tag</b>
					</font>
				</p>
				<hr>
			</td>
		</tr>
		<!-- 标签列表 -->
		<tr>
			<td>
			<table width="80%" align="center" cellpadding="5" cellspacing="5">
				<tbody>
					<tr>
						<td width="25%" align="center">
						</td>
						<td width="50%" align="left">
							<ul>
								{% for tag in site.tags %}
									<li>
										<a href="/ta/ta-{{ tag[0] }}">
										{{ tag[0] }}
										</a>
									</li>
								{% endfor %}
							</ul>
						</td>
						<td width="25%" align="center">
						</td>
					</tr>
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
