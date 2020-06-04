# JavaPrograms
My First Github Repository


```JAVA

package com.practice.test;

import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.ArrayList;
import java.util.Scanner;
import java.util.stream.Collectors;

import com.google.gson.Gson;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;


public class ReadAPI {


	public static void main(String[] args) throws IOException{
		
		String titleName = "spiderman";
		
		URL url = new URL("https://jsonmock.hackerrank.com/api/movies/search/?Title="+titleName);
		
		HttpURLConnection conn = (HttpURLConnection) url.openConnection();
		
		conn.setRequestMethod("GET");
		
		conn.connect();
		
		int responseCode = conn.getResponseCode();
		
		ArrayList al = new ArrayList();
	
		
		if(responseCode!=200) {
			throw new RuntimeException("HttpResponseCode : "+responseCode);
		}else {
			Scanner sc = new Scanner(url.openStream());
			String jsonString = sc.nextLine();
			JsonObject jObj = new Gson().fromJson(jsonString, JsonObject.class);
			int totalPages = jObj.get("total_pages").getAsInt();
			for(int i = 1 ; i <= totalPages; i++) {
				
				url = new URL("https://jsonmock.hackerrank.com/api/movies/search/?Title="+titleName+"&page="+i);
				sc = new Scanner(url.openStream());
				JsonArray jArray = new Gson().fromJson(sc.nextLine(),JsonObject.class).get("data").getAsJsonArray();
				for(int j = 0; j < jArray.size(); j++) {
					al.add(new Gson().fromJson(jArray.get(j), JsonObject.class).get("Title").toString());
				}
				
				
			}
			
			System.out.println(al);
			
			System.out.println(al.stream().sorted().collect(Collectors.toList()));
			
			System.out.println(al.stream().sorted((s1,s2)->s2.toString().compareTo(s1.toString())).collect(Collectors.toList()));
			
			
		}
			
		
	}
		

}


````
