一些简单的crud，可以使用mybatis-generator自动生成，或手写



# 批量删除

~~~xml
  <!--批量删除  -->
  <delete id="batchDeleteVegetableGrowLogFile" parameterType="int[]">
    delete from  t_logfile  where id IN
    <foreach collection="array" item="id" open="(" separator=", " close=")">
      #{id}
    </foreach>
  </delete>
~~~



# 多表连接

~~~xml
  <resultMap type="com.hc.znsc.model.Greenhouse" id="myClass">
    <id property="id" column="id"/>
    <result property="name" column="tgname"/>
    <result property="waterRate" column="water_rate"/>
    <result property="electricityCharge" column="electricity_charge"/>
    <result property="status" column="status"/>
    <!--
        通过association 元素实现一对一级联映射
            property ：当前类中  关联的实体类的    属性名
            javaType ：该属性的类型
    -->
    <association property="vegetablePlantings" javaType="com.hc.znsc.model.VegetablePlanting">
      <result property="name" column="tvname"/>
      <result property="startTime" column="start_time"/>
      <result property="endTime" column="end_time"/>
      <result property="vegetableStatus" column="vegetable_status"/>
      <result property="greenhouse" column="greenhouse"/>
      <result property="space" column="space"/>
    </association>
  </resultMap>

  <select id="searchGreenhouse" resultMap="myClass" parameterType="String">
		select tg.name as tgname,tg.*,tv.name as tvname,tv.* 
      	from t_greenhouse as tg,t_vegetableplanting as tv
		where tg.name like "%"#{name}"%" and tv.greenhouse = tg.name

	</select>
~~~



# 查询某个时间段

查询某个时间段的数据，其中 start_time是mysql里面的字段名，startTime是实体类的属性名

~~~xml
  <select id="searchWaterElectricityInformation" resultMap="BaseResultMap">
    select * from t_logwaterelectricity
    <where>
      <if test="greenhouse != null">
        greenhouse=#{greenhouse}
      </if>
      <if test="startTime != null">
        and start_time <![CDATA[>=]]> #{startTime}
      </if>
      <if test="endTime != null">

        and end_time <![CDATA[<=]]> #{endTime}
      </if>

    </where>
  </select>
~~~

