# Js
利用JQuery将JSON数据进行前端分分页




function getJSONData(pn) {  
	// alert(pn);  
	
	 $.get("../assist/assistFindAll",null,callback);
	function callback(data) {  
		var totalCount =getJsonObjLength(data.assists); // 总记录数  
		var pageSize = 10; // 每页显示几条记录  
		var pageTotal = Math.ceil(totalCount / pageSize); // 总页数  
		var startPage = pageSize * (pn - 1);  
		var endPage = startPage + pageSize - 1;  
		var ul = $(".list");  
		ul.empty();  
		for (var i = 0; i < pageSize; i++) {  
			ul.append('<li></li>');  
		}  
		var dataRoot = data.assists;  
		if (pageTotal == 1) {     // 当只有一页时  
			for (var j = 0; j < totalCount; j++) {  
				$("li").eq(j).append("<div class='ttl'>" +"<a href=\"update.html?id="+dataRoot[j].id+"\">" +dataRoot[j].question+"</a>"
								+ "</div>").append("<div class='timestamp'>" + dataRoot[j].pdate
										+"</div>").append("<div style=\"clear:both\">"
												+ "</div>").append("<span>" + dataRoot[j].reponse  
														+ "</span>");
			}  
		} else {  
			for (var j = startPage, k = 0; j < endPage, k < pageSize; j++, k++) {  
				if( j == totalCount){  
					break;       // 当遍历到最后一条记录时，跳出循环  
				}  
				$("li").eq(k).append("<div class='ttl'>" +"<a href=\"update.html?id="+dataRoot[j].id+"\">" +dataRoot[j].question+"</a>"  
								+ "</div>").append("<div class='timestamp'>" + dataRoot[j].pdate
										+"</div>").append("<div style=\"clear:both\">"
												+ "</div>").append("<span>" + dataRoot[j].reponse  
														+ "</span>");
			}  
		}  
		$(".page-count").text("共"+pageTotal+"页");  
	}
}  
function getPage() {  

	 $.get("../assist/assistFindAll",null,callback);

	function callback(data) {  
		pn = 1;  
		var totalCount = getJsonObjLength(data.assists); // 总记录数  
		var pageSize = 10; // 每页显示几条记录  
		var pageTotal = Math.ceil(totalCount / pageSize); // 总页数  
		$("#next").click(function() {  
			if (pn == pageTotal) {  
				alert("后面没有了");  
				pn = pageTotal;  
			} else {  
				pn++;  
				gotoPage(pn);  
			}  
		});  
		$("#prev").click(function() {  
			if (pn == 1) {  
				alert("前面没有了");  
				pn = 1;  
			} else {  
				pn--;  
				gotoPage(pn);  
			}  
		})  ;
		$("#firstPage").click(function() {  
			pn = 1;  
			gotoPage(pn);  
		});  
		$("#lastPage").click(function() {  
			pn = pageTotal;  
			gotoPage(pn);  
		});  
		$("#page-jump").click(function(){  
			if($(".page-num").val()  <= pageTotal && $(".page-num").val() != ''){  
				pn = $(".page-num").val();  
				gotoPage(pn);  
			}else{  
				alert("您输入的页码有误！");  
				$(".page-num").val('').focus();  
			}  
		})  ;
		$("#firstPage").trigger("click");  


	}  

}
function gotoPage(pn) {  
	// alert(pn);  
	$(".current-page").text("第"+pn+"页");  
	getJSONData(pn) ;
}  

