<template>
    <div class="article-add">
      <articleEditor @articleHandle="articleAdd" />
    </div>
</template>
<script>
import articleEditor from '@/components/articleEditor'
export default {
  name: 'ArticleAdd',
  components: {
    articleEditor
  },
  data () {
    return {
    }
  },
  methods: {
    articleAdd (articleData, successCallBack) {
      this.$http.createArticles(articleData).then(res => {
        if (res.code === 0) {
          const { title } = articleData
          this.$notify({
            title: '添加成功',
            message: '文章' + '《' + title + '》' + '添加成功',
            type: 'success'
          })
          successCallBack()
          return this.$router.replace('/main/article')
        } else {
          return this.$message({
            message: res.message,
            type: 'warning'
          })
        }
      }).catch(err => {
        this.$message.error(err.message)
      })
    }
  }
}
</script>
