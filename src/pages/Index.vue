<template>
	<Layout>
		<!-- Page Header -->
		<header
			class="masthead"
			:style="{
				backgroundImage: `url(${GRIDSOME_API_URL}${$page.general.edges[0].node.cover.url})`,
			}"
		>
			<div class="overlay"></div>
			<div class="container">
				<div class="row">
					<div class="col-lg-8 col-md-10 mx-auto">
						<div class="site-heading">
							<h1>{{ $page.general.edges[0].node['title'] }}</h1>
							<span class="subheading">
								{{ $page.general.edges[0].node['subtitle'] }}
							</span>
						</div>
					</div>
				</div>
			</div>
		</header>
		<!-- Main Content -->
		<div class="container">
			<div class="row">
				<div class="col-lg-8 col-md-10 mx-auto">
					<div class="post-preview" v-for="item in $page.posts.edges" :key="item.node.id">
						<g-link :to="`/post/${item.node.id}`">
							<h2 class="post-title">
								{{ item.node.title }}
							</h2>
						</g-link>
						<p>{{ item.node.content }}</p>

						<p class="post-meta">
							Posted by
							<a href="#">{{ item.node.creator }}</a>
							on {{ item.node.created_at }}
						</p>
						<p>
							<g-link
								:to="`/tag/${tag.id}`"
								v-for="tag in item.node.tags"
								:key="tag.id"
								>{{ tag.title }}</g-link
							>
						</p>
						<hr />
					</div>
					<Pager :info="$page.posts.pageInfo" />
				</div>
			</div>
		</div>
	</Layout>
</template>
<page-query>
query ($page: Int) {
  posts:allStrapiPost (perPage:2,page:$page) @paginate {
		pageInfo {
      totalPages
      currentPage
    }
    edges{
      node{
        id
				title
        tags{
          id
          title
        }
				creator
				content
				created_at
      }
    }
  },
	general:allStrapiGeneral{
    edges{
      node{
        id
				title
				subtitle
				cover{
					url
				}
      }
    }
  }
}
</page-query>
<script>
import { Pager } from 'gridsome'

export default {
	name: 'IndexPage',
	metaInfo: {
		title: 'Hello, world!',
	},
	components: {
		Pager,
	},
}
</script>

<style></style>
