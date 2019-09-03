---
tags: [Sails]
title: sql-to-model
created: '2019-07-02T01:42:07.372Z'
modified: '2019-09-03T09:44:25.377Z'
---

自动生成 sails.js 数据库操作模型
原创 2017年12月15日 15:56:31 标签：sails-js 62
最近接手了兄弟部门的一个项目，前端是用 基于 Vue 的 nuxt.js，后端用的是 Node 的 sails.js，该框架默认用 Waterline 的 ORM 操作数据库，思想和 MyBatis 的类似，生成数据表的实体，然后取出数据映射到实体，进而操作实体。由于每张表都需要生成对应的实体文件，一个个的写肯定不符合程序员的思维，以下给出个简单的自动生成的脚本类

首先是数据表
CREATE TABLE `TAB_TEL_STATUS` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `status` tinyint(1) NOT NULL DEFAULT 1 COMMENT '状态 1签出 2签入',
  `seat` varchar(20) NOT NULL COMMENT '坐席',
  `staffId` varchar(20) NOT NULL COMMENT '人员编码',
  `email` varchar(255) NOT NULL DEFAULT '' COMMENT '公司邮箱',
  `name` varchar(50) NOT NULL DEFAULT '' COMMENT '人员姓名',
  `createdAt` datetime DEFAULT CURRENT_TIMESTAMP,
  `updatedAt` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_seat` (`seat`),
  KEY `idx_email` (`email`),
  KEY `idx_staffId` (`staffId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='坐席状态表';
1
2
3
4
5
6
7
8
9
10
11
12
13
14
其次是自动生成的脚本
<?php

class generate
{
    public function __construct($host, $name, $pwd, $table)
    {
        $conn = mysqli_connect($host, $name, $pwd);
        mysqli_select_db($conn, 'fortune');
        mysqli_query($conn, 'SET NAMES utf8'); 
        $result = mysqli_query($conn, 'SHOW FULL COLUMNS FROM ' . $table);
        $data = [];
        while ($row = mysqli_fetch_assoc($result)) {
            $data[] = $row;
        }
        $attributes = $this->getAttributes($data);
        $str = <<<EOT
// this file is auto generated via php script

'use strict'

module.exports = {
    tableName: '{$table}',
{$attributes['attributes']}
{$attributes['_attributes']}
}
EOT;

        file_put_contents($this->convertTitle($table) . '.js', $str);
    }

    public function getAttributes($lists)
    {
        $delimiter = PHP_EOL;
        $attributes = <<<EOT
    attributes: {{$delimiter}
EOT;
$_attributes = <<<EOT
    _attributes: {{$delimiter}
EOT;
    foreach ($lists as $value) {
        $type = $this->get(strpos($value['Type'], '(')=== false ? $value['Type'] : substr($value['Type'], 0, strpos($value['Type'], '(')));
        $rawType = strpos($value['Type'], ' ')=== false ? $value['Type'] : substr($value['Type'], 0, strpos($value['Type'], ' '));
        $columnName = strpos($value['Field'], '_') !== false ? $this->convertUnderline($value['Field'], false) : $value['Field'];
        if ($value['Key'] == 'PRI' && $value['Extra'] == 'auto_increment') {
            $attributes .= <<<EOT
        {$value['Field']}: {
            type: '{$type}',
            primaryKey: true,
            autoIncrement: true
        },{$delimiter}
EOT;
            $_attributes .= <<<EOT
        {$value['Field']}: {
            rawType: '{$rawType}',
            type: '{$type}',
            primaryKey: true,
            autoIncrement: true
        },{$delimiter}
EOT;
        } else {
            $attributes .= <<<EOT
        {$value['Field']}: {
            type: '{$type}'
        },{$delimiter}
EOT;
        if (strpos($value['Field'], '_') === false) {
            $_attributes .= <<<EOT
        {$value['Field']}: {
            rawType: '{$rawType}',
            type: '{$type}',
            comment: '{$value['Comment']}'
        },{$delimiter}
EOT;
            } else {
    $_attributes .= <<<EOT
        {$value['Field']}: {
            columnName: '{$columnName}',
            rawType: '{$rawType}',
            type: '{$type}',
            comment: '{$value['Comment']}'
        },{$delimiter}
EOT;
                }
            }
        }
        $attributes = rtrim($attributes, ",{$delimiter}") . "{$delimiter}    },";
        $_attributes = rtrim($_attributes, ",{$delimiter}") . "{$delimiter}    }";
        return [
            'attributes' => $attributes,
            '_attributes' => $_attributes
        ];
    }

    public function get($key)
    {
        $arr = [
            'smallint'     => 'integer',
            'int'              => 'integer',
            'varchar'      => 'string',
            'tinyint'        => 'integer',
            'datetime'    => 'datetime'
        ];
        return $arr[$key];
    }

    public function _ucfirst(&$value)
    {
        $value = ucfirst($value);
    }

    public function convertTitle($name)
    {
        $arr =  explode('_', strtolower($name));
        array_walk($arr, [$this, '_ucfirst']);
        return implode('', $arr);
    }
}

new generate('localhost', 'admin', 'admin', 'TAB_TEL_STATUS');
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
最后，自动生成后的 model 为 TabTelStatus.js
// this file is auto generated via php script

'use strict'

module.exports = {
    tableName: 'TAB_TEL_STATUS',
    attributes: {
        id: {
            type: 'integer',
            primaryKey: true,
            autoIncrement: true
        },
        status: {
            type: 'integer'
        },
        seat: {
            type: 'string'
        },
        staffId: {
            type: 'string'
        },
        email: {
            type: 'string'
        },
        name: {
            type: 'string'
        },
        createdAt: {
            type: 'datetime'
        },
        updatedAt: {
            type: 'datetime'
        }
    },
    _attributes: {
        id: {
            rawType: 'int(11)',
            type: 'integer',
            primaryKey: true,
            autoIncrement: true
        },
        status: {
            rawType: 'tinyint(1)',
            type: 'integer',
            comment: '状态 1签出 2签入'
        },
        seat: {
            rawType: 'varchar(20)',
            type: 'string',
            comment: '坐席'
        },
        staffId: {
            rawType: 'varchar(20)',
            type: 'string',
            comment: '人员编码'
        },
        email: {
            rawType: 'varchar(255)',
            type: 'string',
            comment: '公司邮箱'
        },
        name: {
            rawType: 'varchar(50)',
            type: 'string',
            comment: '人员姓名'
        },
        createdAt: {
            rawType: 'datetime',
            type: 'datetime',
            comment: ''
        },
        updatedAt: {
            rawType: 'datetime',
            type: 'datetime',
            comment: ''
        }
    }
}
